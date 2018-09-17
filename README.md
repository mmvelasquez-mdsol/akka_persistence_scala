# Akka Persistence example with MySql akka-persistence-sql-async Plugin
Akka Persistence with MySQL configured as Event Store.
 - Akka Persistence (Write): YES
 - Akka Persistence Query (Read): NO  

Plugin Name: akka-persistence-sql-async
Plugin URL: https://index.scala-lang.org/okumin/akka-persistence-sql-async/akka-persistence-sql-async/0.5.1

# Create Database
```
CREATE DATABASE akka DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;
GRANT ALL PRIVILEGES ON akka.* TO akka@localhost IDENTIFIED BY 'akka' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

# Create Event Store tables    
```
CREATE TABLE IF NOT EXISTS persistence_metadata (
  persistence_key BIGINT NOT NULL AUTO_INCREMENT,
  persistence_id VARCHAR(255) NOT NULL,
  sequence_nr BIGINT NOT NULL,
  PRIMARY KEY (persistence_key),
  UNIQUE (persistence_id)
) ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS persistence_journal (
  persistence_key BIGINT NOT NULL,
  sequence_nr BIGINT NOT NULL,
  message LONGBLOB NOT NULL,
  PRIMARY KEY (persistence_key, sequence_nr),
  FOREIGN KEY (persistence_key) REFERENCES persistence_metadata (persistence_key)
) ENGINE = InnoDB;

CREATE TABLE IF NOT EXISTS persistence_snapshot (
  persistence_key BIGINT NOT NULL,
  sequence_nr BIGINT NOT NULL,
  created_at BIGINT NOT NULL,
  snapshot LONGBLOB NOT NULL,
  PRIMARY KEY (persistence_key, sequence_nr),
  FOREIGN KEY (persistence_key) REFERENCES persistence_metadata (persistence_key)
) ENGINE = InnoDB;
```

