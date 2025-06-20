Этот набор таблиц уже покрывает:
- Аутентификацию и авторизацию (`users`, `sessions`)
- Управление пользовательским контентом (`reminders`, `categories`)
- Аудит и историю действий (`activity_logs`, `moderation_logs`)
- Гибкость с ролями и расширением (через `jsonb`, `role`, `metadata`, `details`)

```sql
-- Migrations will appear here as you chat with AI

  
create table users (
  id bigint primary key generated always as identity,
  username text not null unique,
  email text not null unique,
  password text not null,
  avatar text,
  role text not null check (role in ('admin', 'moderator', 'standard'))
);

create table categories (
  id bigint primary key generated always as identity,
  user_id bigint not null references users (id) on delete cascade,
  name text not null
);

create table reminders (
  id bigint primary key generated always as identity,
  user_id bigint not null references users (id) on delete cascade,
  category_id bigint references categories (id) on delete set null,
  message text not null,
  created_at timestamp with time zone default now()
);


create table activity_logs (
  id bigint primary key generated always as identity,
  user_id bigint not null references users (id) on delete cascade,
  action_type text not null,
  "timestamp" timestamp with time zone default now(),
  metadata jsonb
);

create table sessions (
  id bigint primary key generated always as identity,
  user_id bigint not null references users (id) on delete cascade,
  session_token text not null unique,
  device_info text,
  expires_at timestamp with time zone not null
);

create table moderation_logs (
  id bigint primary key generated always as identity,
  admin_id bigint not null references users (id) on delete cascade,
  action_type text not null,
  "timestamp" timestamp with time zone default now(),
  details jsonb
);
```

![[photo_2025-06-15_13-59-32.jpg]]