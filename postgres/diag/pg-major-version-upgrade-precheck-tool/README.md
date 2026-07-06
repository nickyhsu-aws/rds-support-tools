# Aurora / RDS for PostgreSQL Major Version Upgrade Pre-Check Tool

> AWS pre-upgrade check tool for **Amazon Aurora PostgreSQL** and
> **Amazon RDS for PostgreSQL**, maintained under
> [awslabs/rds-support-tools](https://github.com/awslabs/rds-support-tools).

This tool helps you **identify potential issues before a PostgreSQL major
version upgrade**. It performs a comprehensive set of pre-upgrade readiness
checks across every database in a cluster so you can find and fix blockers
before you run the upgrade, and validate readiness for **Blue/Green
deployments**.

**Keywords:** PostgreSQL pre-upgrade check, RDS PostgreSQL major version
upgrade, Aurora PostgreSQL upgrade precheck, pg_upgrade compatibility check,
Blue/Green deployment readiness.

## Why use this tool

- Runs **51 comprehensive checks** to validate major version upgrade readiness.
- Detects upgrade **blockers** (unsupported data types, incompatible
  extensions, `reg*` types, unknown-type columns, and more) before they cause a
  failed or rolled-back upgrade.
- Validates **Blue/Green deployment** prerequisites (logical replication
  settings, sequences, unsupported features).
- Analyzes RDS/Aurora configuration and CloudWatch metrics.
- Works against **Aurora/RDS PostgreSQL 11–18**.

## Available versions

| Version | Path | Best for |
| --- | --- | --- |
| **Shell script** | [`shell/`](./shell) | Full automation, cross-database iteration, HTML/text reports, AWS Secrets Manager, batch runs via `wrapper.sh` |
| **SQL (psql)** | [`sql/`](./sql) | Pure `psql` meta-command script, no shell required, run per database |
| **PL/pgSQL** | [`plpgsql/`](./plpgsql) | Callable function from any PostgreSQL client, run per database |

## Quick start (shell version)

Interactive:

```bash
bash pg-major-version-upgrade-precheck.sh
```

Non-interactive:

```bash
bash pg-major-version-upgrade-precheck.sh --non-interactive -m sql \
    -h <endpoint> -P <port> -d <database> -u <user> -w <password>
```

With Blue/Green checks and both check modes:

```bash
bash pg-major-version-upgrade-precheck.sh --non-interactive --blue-green -m both \
    -r <region> -i <identifier> -p <profile> \
    -h <endpoint> -P <port> -d <database> -u <user> -s <secret-arn>
```

Output: `rds_precheck_report_<identifier>_<timestamp>.html` (or `.txt`).

Prerequisites: `bash 4.0+`, `psql`, `aws cli` (for RDS mode), `jq`.

## What it checks

- **25 standard pre-upgrade checks** (always executed)
- **14 Blue/Green deployment checks** (optional, `--blue-green`)
- **12 critical upgrade blocker checks** (always executed, version-conditional)
- Cross-database iteration for extension, data type, and compatibility checks
- Instance-wide `max_locks_per_transaction` calculation with a 20% safety buffer

## Related AWS resources

- [PostgreSQL on Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html)
- [Upgrading the PostgreSQL DB engine for Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_UpgradeDBInstance.PostgreSQL.html)
- [Upgrades for Amazon Aurora PostgreSQL](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/USER_UpgradeDBInstance.PostgreSQL.html)
- [Blue/Green Deployments for Amazon RDS](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/blue-green-deployments.html)

## License

Licensed under the Apache License, Version 2.0. See the repository
[LICENSE](https://github.com/awslabs/rds-support-tools/blob/main/LICENSE).
