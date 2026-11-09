# ORA-N2-01 — Sessions & verrous

**Quand ?** Timeouts, attente de verrou, blocage applicatif.

### Paires session bloquée / bloqueur
```sql
SELECT sid, sess_serial#, blocker_instance_id,
       blocker_sid, blocker_sess_serial#, wait_event_text
FROM v$session_blockers
FETCH FIRST 20 ROWS ONLY;
```

### Sessions en attente
```sql
SELECT sid, serial#, con_id, event, sql_id,
       blocking_instance, blocking_session
FROM v$session
WHERE blocking_session_status = 'VALID'
FETCH FIRST 20 ROWS ONLY;
```

### Lecture et action
- `BLOCKER_SID` **et** `BLOCKER_INSTANCE_ID` identifient le bloqueur (important en RAC).
- Relevé ponctuel : une sortie vide n'exclut pas un blocage intermittent.
- **Action N2 :** confirmer propriétaire, transaction, rôle applicatif et conséquence d'une interruption ; annulation de requête/session uniquement après autorisation et procédure.
- **Validation :** attente disparue, transactions stabilisées et service fonctionnel. Une session interrompue ne peut pas être rétablie par un rollback DBA.

**Escalade N3 :** chaîne RAC complexe, transaction critique ou annulation risquée.

[Sources : V$SESSION_BLOCKERS](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-SESSION_BLOCKERS.html) · [V$SESSION](https://docs.oracle.com/en/database/oracle/oracle-database/19/refrn/V-SESSION.html)
