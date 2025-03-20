# 1. DB API specsheet
Time: 20-03-2025 @ 08:31
Author: Sebastian Lindau-Skands @ GNUF | Backend
Reciever: GNUF | DB Team
Version: 2.0

# 2. Overview
DB: postgress
Env: postgres/postgres:17.4-alpine3.21
Dedicated com-port for DB: 9012
USERNAME: gnufdb
PASSWORD: 123456789

# 3. Database Schema
  ## 3.1 Community
  ```
  Column name    Type       Constraints                                     Description
  -------------- ---------- ----------------------------------------------- --------------------------------------
  ID             UUID       PRIMARY KEY                                     Unique Identifier
  NAME           TEXT       UNIQUE. NOT NULL. CHECK (LENGTH(NAME) \< 100)   Community Name
  Description    TEXT       CHECK (LENGTH(description) \< 500)              Community description
  IMG_PATH       TEXT                                                       URL to community image
  MEMBER_COUNT   INT        DEFAULT 0. NOT NULL                             Number of members
  TAGS           UUID\[\]   NOT NULL                                        Content tags
  POST_IDs       UUID\[\]                                                   Array of post ID\'s (FK to posts.id)
  ```

## 3.2 User
  ```
  Column Name     Type       Constraints                                         Description
  --------------- ---------- --------------------------------------------------- ---------------------------------------------
  ID              UUID       PRIMARY KEY                                         Unique Identifier
  EMAIL           TEXT       UNIQUE. NOT NULL                                    User\'s Email
  USERNAME        TEXT       UNIQUE. NOT NULL. CHECK (LENGTH(USERNAME) \< 100)   Unique Username
  PASSWORD        TEXT       NOT NULL. CHECK (LENGTH(PASSWORD) \< 1000)          Hashed Password (sha256)
  IMG_PATH        TEXT                                                           URL TO PP
  POST_IDs        UUID\[\]                                                       Array of post IDs (FK to posts.id)
  COMMUNITY_IDs   UUID\[\]                                                       Array of communities (FK to communities.id)
  ADMIN           BOOL       NOT NULL. DEFAULT FALSE                             ADMIN FLAG
  TAGS            UUID\[\]                                                       Array of tags for content recommendation
  ```

## 3.3 Posts
  ```
  Column Name    Type        Constraints                       Description
  -------------- ----------- --------------------------------- ---------------------------------------------------------
  POST_ID        UUID        UNIQUE. NOT NULL                  Unique Identifier
  TITLE          TEXT        NOT NULL. CHECK (LENGHT \< 100)   Post title
  MAIN_TEXT      TEXT        CHECK (LENGTH \< 10k)             Post body
  AUTH_ID        UUID        NOT NULL                          Author ID (FK. User.ID)
  COM_ID         UUID        NOT NULL                          Community ID (FK. Community.ID)
  TIMESTAMP      TIMESTAMP   NOT NULL                          Time of post
  LIKES          INT         NOT NULL. DEFAULT 0               Likes on post
  DISLIKES       INT         NOT NULL. DEFAULT 0               Dislikes on post
  POST_ID_REF    UUID                                          Refference to original post (for reposts) (FK. Post.ID)
  COMMENT_FLAG   BOOL        NOT NULL                          Indicates comment instead of post
  COMMENT_CNT    INT         NOT NULL. DEFAULT 0               Comment count
  Comments       UUID\[\]                                      Array of post id\'s for comments (FK. Post.ID)
  ```
if POST_ID_REF is set, this indicates repost by default UNLESS COMMENT_FLAG is set too. COMMENT_FLAG cannot be set without POST_ID_REF
