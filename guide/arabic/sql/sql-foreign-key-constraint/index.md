---
title: SQL Foreign Key Constraint
localeTitle: SQL الخارجية مفتاح القيد
---
## SQL الخارجية مفتاح القيد

المفتاح الخارجي هو مفتاح يستخدم لربط جدولين. يتم ربط الجدول مع "مفتاح خارجي" (الملقب "جدول الطفل") إلى جدول آخر (aka ، "الجدول الأصل"). يقع الاتصال بين قيد المفتاح الخارجي في الجدول الفرعي والمفتاح الأساسي للجدول الأصل.

يتم استخدام قيود المفتاح الخارجي للمساعدة في الحفاظ على التناسق بين الجداول. على سبيل المثال ، إذا تم حذف سجل الجدول الأصل وكان الجدول الفرعي يحتوي على سجلات ، يمكن للنظام أيضًا حذف السجلات الفرعية.

كما أنها تساعد على منع إدخال بيانات غير دقيقة في الجدول التابع من خلال المطالبة بسجل جدول أساسي لكل سجل تم إدخاله في الجدول التابع.

### مثال على الاستخدام

بالنسبة إلى هذا الدليل ، سنلقي نظرة عن قرب على جداول الطلاب (الوالدين) وطالب الاتصال (الطفل).

### المفتاح الأساسي للجدول الأصل

لاحظ أن جدول الطالب يحتوي على مفتاح أساسي عمود واحد للطالب.

```sql
SHOW index FROM student;
``` 

```text
+---------+------------+----------+--------------+-------------+
| Table   | Non_unique | Key_name | Seq_in_index | Column_name |
+---------+------------+----------+--------------+-------------+
| student |          0 | PRIMARY  |            1 | studentID   |
+---------+------------+----------+--------------+-------------+
1 row in set (0.00 sec) (some columns removed on the right for clarity)
``` 

### مفاتيح الجدول الأساسي والأجنبي للطفل

يحتوي جدول معلومات الاتصال الخاصة بالطالب على مفتاح أساسي واحد وهو أيضًا الطالب الطالب. هذا لأن هناك علاقة رأس برأس بين الجدولين. وبعبارة أخرى ، فإننا نتوقع أن يكون هناك طالب واحد فقط وسجل طالب واحد لكل طالب.

 ``SHOW index FROM `student-contact-info`; 
`` 

```text
+----------------------+------------+----------+--------------+-------------+
| Table                | Non_unique | Key_name | Seq_in_index | Column_name |
+----------------------+------------+----------+--------------+-------------+
| student-contact-info |          0 | PRIMARY  |            1 | studentID   |
+----------------------+------------+----------+--------------+-------------+
1 row in set (0.00 sec) (some columns removed on the right for clarity)
``` 

```sql
SELECT concat(table_name, '.', column_name) AS 'foreign key',
concat(referenced_table_name, '.', referenced_column_name) AS 'references'
FROM information_schema.key_column_usage
WHERE referenced_table_name IS NOT NULL
AND table_schema = 'fcc_sql_guides_database'
AND table_name = 'student-contact-info';
``` 

```text
+--------------------------------+-------------------+
| foreign key                    | references        |
+--------------------------------+-------------------+
| student-contact-info.studentID | student.studentID |
+--------------------------------+-------------------+
1 row in set (0.00 sec)
``` 

### مثال على التقرير باستخدام جدول الطالب الرئيسي والجدول الفرعي للاتصال

 ``SELECT a.studentID, a.FullName, a.programOfStudy, 
 b.`student-phone-cell`, b.`student-US-zipcode` 
 FROM student AS a 
 JOIN `student-contact-info` AS b ON a.studentID = b.studentID; 
`` 

```text
+-----------+------------------------+------------------+--------------------+--------------------+
| studentID | FullName               | programOfStudy   | student-phone-cell | student-US-zipcode |
+-----------+------------------------+------------------+--------------------+--------------------+
|         1 | Monique Davis          | Literature       | 555-555-5551       |              97111 |
|         2 | Teri Gutierrez         | Programming      | 555-555-5552       |              97112 |
|         3 | Spencer Pautier        | Programming      | 555-555-5553       |              97113 |
|         4 | Louis Ramsey           | Programming      | 555-555-5554       |              97114 |
|         5 | Alvin Greene           | Programming      | 555-555-5555       |              97115 |
|         6 | Sophie Freeman         | Programming      | 555-555-5556       |              97116 |
|         7 | Edgar Frank "Ted" Codd | Computer Science | 555-555-5557       |              97117 |
|         8 | Donald D. Chamberlin   | Computer Science | 555-555-5558       |              97118 |
+-----------+------------------------+------------------+--------------------+--------------------+
``` 

### استنتاج

قيود المفتاح الخارجي هي أداة تكامل بيانات رائعة. خذ الوقت الكافي لتعلمها جيداً.

كما هو الحال مع كل هذه الأشياء SQL هناك أكثر من ذلك بكثير من ما هو موجود في هذا الدليل التمهيدي.

آمل أن يمنحك هذا على الأقل ما يكفي للبدء.

يرجى الاطلاع على دليل مدير قاعدة البيانات الخاص بك والمتعة محاولة خيارات مختلفة بنفسك.