# Day 17 — PostgreSQL Database & User Setup

## Platform
KodeKloud Engineer — Nautilus Application Development

## Task Description

The `Nautilus` application development team is planning to deploy a newly developed application on `Nautilus` infra in `Stratos DC`. The application uses PostgreSQL database, so as a pre-requisite we need to set up the PostgreSQL database server.

PostgreSQL database server is **already installed** on the `Nautilus` database server.

## Requirements

- **a.** Create a database user `kodekloud_tim` and set its password to `YchZHRcLkL`.
- **b.** Create a database `kodekloud_db5` and grant full permissions to user `kodekloud_tim` on this database.

## Constraint

> ⚠️ Do **NOT** restart the PostgreSQL server service.

## Environment

| Host     | Role              | User    | Password     |
|----------|-------------------|---------|--------------|
| stdb01   | Database Server   | peter   | Sp!dy  |
