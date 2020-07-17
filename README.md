# SeciossOAuth
SeciossOAuth は、以下の機能を提供するオープンソースのソフトウェアです。
* OAuth 2.0
* OpenID Connect 1.0

## 目標
* OAuth 2.0
* OpenID Connect 1.0

## インストール
### 環境
* OS：CentOS 7
* ミドルウェア：Apache、PHP、MariaDB

### インストール
packageを展開して、インストールスクリプトを実行。
# ./isntall.sh install`

### データベースのインストール
MariaDBをインストールして、以下のテーブルを作成して下さい。
~~~
CREATE DATABASE `oauth_db` DEFAULT CHARACTER SET utf8;

CREATE  TABLE `oauth_db`.`auth_code_tbl` (
  `code` CHAR(60) NOT NULL,
  `client_id` VARCHAR(255) NOT NULL,
  `redirect_uri` VARCHAR(255) NOT NULL,
  `username` VARCHAR(255) NOT NULL,
  `expires` CHAR(10) NOT NULL,
  `scope` VARCHAR(255) NULL DEFAULT NULL,
  `additional_data` TEXT NULL DEFAULT NULL,
  `nonce` VARCHAR(255) NULL DEFAULT NULL,
  `claims` VARCHAR(255) NULL DEFAULT NULL,
  PRIMARY KEY (`code`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE  TABLE `oauth_db`.`access_token_tbl` (
  `access_token` CHAR(60) NOT NULL,
  `client_id` VARCHAR(255) NOT NULL,
  `username` VARCHAR(255) NOT NULL,
  `expires` CHAR(10) NOT NULL,
  `scope` VARCHAR(255) NULL DEFAULT NULL,
  `additional_data` TEXT NULL DEFAULT NULL,
  `nonce` VARCHAR(255) NULL DEFAULT NULL,
  `claims` VARCHAR(255) NULL DEFAULT NULL,
  PRIMARY KEY (`access_token`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE  TABLE `oauth_db`.`refresh_token_tbl` (
  `refresh_token` CHAR(60) NOT NULL,
  `access_token` CHAR(60) NOT NULL,
  `client_id` VARCHAR(255) NOT NULL,
  `username` VARCHAR(255) NOT NULL,
  `expires` CHAR(10) NOT NULL,
  `scope` VARCHAR(255) NULL DEFAULT NULL,
  `additional_data` TEXT NULL DEFAULT NULL,
  `nonce` VARCHAR(255) NULL DEFAULT NULL,
  `claims` VARCHAR(255) NULL DEFAULT NULL,
  PRIMARY KEY (`refresh_token`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `oauth_db`.`oauth_tbl` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` varchar(255) NOT NULL,
  `client_id` varchar(255) NOT NULL,
  `scope` varchar(255) NULL DEFAULT NULL,
  `additional_data` TEXT NULL DEFAULT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` timestamp NULL DEFAULT NULL,
  `access_time` timestamp NULL DEFAULT NULL,
  `deleted` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`,`client_id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `oauth_db`.`oidc_tbl` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` varchar(255) NOT NULL,
  `client_id` varchar(255) NOT NULL,
  `scope` varchar(255) NULL DEFAULT NULL,
  `additional_data` TEXT NULL DEFAULT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` timestamp NULL DEFAULT NULL,
  `access_time` timestamp NULL DEFAULT NULL,
  `deleted` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`,`client_id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `oauth_db`.`user_info` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `user_id` varchar(255) NOT NULL,
  `password` varchar(255) NOT NULL,
  `email` varchar(255) NULL DEFAULT NULL,
  `seciossotpoption` varchar(255) NULL DEFAULT NULL,
  `seciossotpsecretpin` varchar(255) NULL DEFAULT NULL,
  `seciossotpinitsecret` varchar(255) NULL DEFAULT NULL,
  `seciossdeviceidxfido` text,
  `seciossaccountstatus` varchar(10) DEFAULT 'active',
  `additional_data` TEXT NULL DEFAULT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` timestamp NULL DEFAULT NULL,
  `access_time` timestamp NULL DEFAULT NULL,
  `deleted` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `user_id` (`user_id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;


CREATE TABLE `oauth_db`.`client_tbl` (
  `id` varchar(255) NOT NULL,
  `client_id` varchar(255) NOT NULL,
  `client_name` varchar(255) NOT NULL,
  `client_secret` varchar(255) NULL DEFAULT NULL,
  `redirect_uri` varchar(255) NULL DEFAULT NULL,
  `customissuer` varchar(255) NULL DEFAULT NULL,
  `idtokensigalg` varchar(10) NULL DEFAULT NULL,
  `idtokensigsecret` varchar(255) NULL DEFAULT NULL,
  `idtokentype` varchar(10) NULL DEFAULT NULL,
  `additional_data` TEXT NULL DEFAULT NULL,
  `create_time` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `update_time` timestamp NULL DEFAULT NULL,
  `access_time` timestamp NULL DEFAULT NULL,
  `deleted` int(11) NOT NULL DEFAULT '0',
  PRIMARY KEY (`id`),
  KEY `client_id` (`client_id`) ) ENGINE=InnoDB DEFAULT CHARSET=utf8;
~~~      

## 設定
### OAuth 2.0 設定
設定ファイル/usr/share/oauth2/conf/config.iniを環境に合わせて変更して下さい。
```
db_host="<データベースホスト名>"
db_user="<データベースユーザー名>"
db_pass="<データベースパスワード>"
db_name="<データベーススキーマ名>"
......
```

### OpenID Connect 1.0 設定
設定ファイル/usr/share/oidc/conf/config.iniを環境に合わせて変更して下さい。
```
db_host="<データベースホスト名>"
db_user="<データベースユーザー名>"
db_pass="<データベースパスワード>"
db_name="<データベーススキーマ名>"
......
```

## discovery
### OAuth 2.0
~~~
https://localhost/.well-known/oauth-authorization-server
~~~
### OpenID Connect 1.0
~~~
https://localhost/.well-known/openid-configuration
~~~



