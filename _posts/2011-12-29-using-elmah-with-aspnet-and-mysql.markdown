---
layout: post
title: Using ELMAH with ASP.Net and MySQL
tags:
comments: true
---
I’ve been trying to use ELMAH (http://code.google.com/p/elmah/) as a quick-and-dirty logging solution for an ASP.Net project I’ve been working on. The installation went smoothly until I actually logged the first error. The Elmah.axd page would crash with an error when trying to view a logged error:

“There is an unclosed literal string”

After some looking, I actually dove into the MySQL table to find the cause. Apparently some ASP.Net pages contain too much text to be stored in the AllXml column of the “elmah_error” table in a MySQL backed application. The only solution is to change the AllXml column type to “LONGTEXT”, then change the “elmah_LogError” procedure so that the IN parameter is also LONGTEXT. Here is the procedure below:

```sql
— —————————————————————————
— Routine DDL
— Note: comments before and after the routine body will not be stored by the server
— —————————————————————————-

DELIMITER $$

CREATE DEFINER=`yourusername`@`%` PROCEDURE `elmah_LogError`(
    IN ErrorId CHAR(36),
    IN Application varchar(60),
    IN Host VARCHAR(30),
    IN Type VARCHAR(100),
    IN Source VARCHAR(60),
    IN Message VARCHAR(500),
    IN User VARCHAR(50),
    IN AllXml LONGTEXT,
    IN StatusCode INT(10),
    IN TimeUtc DATETIME
)

MODIFIES SQL DATA

BEGIN

    INSERT INTO `elmah_error` (
        `ErrorId`,
        `Application`,
        `Host`,
        `Type`,
        `Source`,
        `Message`,
        `User`,
        `AllXml`,
        `StatusCode`,
        `TimeUtc`
    )
    VALUES (
        ErrorId,
        Application,
        Host,
        Type,
        Source,
        Message,
        User,
        AllXml,
        StatusCode,
        TimeUtc
    );

END
```

Changing the column type is easy:
```sql
ALTER TABLE elmah_error modify AllXml LONGTEXT ;
```

Here’s a link to a related issue on the ELMAH site: [http://code.google.com/p/elmah/issues/detail?id=248](http://code.google.com/p/elmah/issues/detail?id=248)
