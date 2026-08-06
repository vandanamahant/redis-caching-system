# Database Schema & ERD Design - Redis Caching

## 📊 Overview
This document outlines the definitive database schema for the Redis Caching system to transition away from legacy Excel sheets.

## 🗄️ Table Structure: `cache_records`

| Column Name | Data Type | Constraints | Description |
| :--- | :--- | :--- | :--- |
| `id` | VARCHAR(36) | PRIMARY KEY (UUIDv4) | Unique identifier for the cache entry |
| `cache_key` | VARCHAR(255) | UNIQUE, NOT NULL, INDEXED | Key used for Redis lookups (e.g., `prop:listing:123`) |
| `cache_value` | JSONB / TEXT | NOT NULL | Sanitized JSON payload storing property data |
| `category` | VARCHAR(100) | INDEXED | Grouping category (e.g., residential, commercial) |
| `ttl_seconds` | INT | DEFAULT 3600 | Time-to-Live expiration window in seconds |
| `status` | VARCHAR(50) | DEFAULT 'active' | Status flags ('active', 'expired', 'invalidated') |
| `created_by` | VARCHAR(100) | NOT NULL | Staff member ID or username |
| `created_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Record creation timestamp |
| `updated_at` | TIMESTAMP | DEFAULT CURRENT_TIMESTAMP | Last modification timestamp |

## 🔗 Relationships & Constraints
- **Indexing:** High-frequency lookups are optimized via indexes on `cache_key` and `category`.
- **Data Integrity:** Strict sanitization is enforced at the application layer prior to database insertion to protect against malicious injections.