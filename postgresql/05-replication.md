# PG-N2-05 — Retard de réplication

**Quand ?** Standby déconnecté, retard ou replay arrêté.

### Sur le primaire
```sql
SELECT application_name, state, sync_state,
       sent_lsn, write_lsn, flush_lsn, replay_lsn,
       write_lag, flush_lag, replay_lag
FROM pg_stat_replication;
```

### Sur le standby
```sql
SELECT status, flushed_lsn, latest_end_lsn,
       last_msg_receipt_time FROM pg_stat_wal_receiver;
SELECT pg_last_wal_receive_lsn(), pg_last_wal_replay_lsn(),
       pg_is_wal_replay_paused();
```

### Lecture et action
- `write_lag`, `flush_lag`, `replay_lag` peuvent être `NULL` : aucune durée ne prédit le rattrapage.
- WAL reçu ≠ WAL rejoué ; comparer les LSN, le réseau, les slots, le stockage et la supervision.
- Un `last_msg_receipt_time` ancien doit être interprété avec l'activité WAL attendue.
- **Action N2 :** qualifier la rupture, proposer correction approuvée, puis contrôler le retour du streaming et du replay. Aucun failover autonome.

**Escalade N3 :** WAL absent, reprise complexe ou risque de perte de données.

[Source : réplication](https://www.postgresql.org/docs/18/monitoring-stats.html)
