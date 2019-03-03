# DatabasesAssignment5

<h2>Introduction</h2>
we used wget and p7zip in order to get the xml files.

apt-get install p7zip-full. to get p7zip...

wget url to get file...
  
7z e <full zip file name> to extract to current folder.

mysql -u myuser -p --local-infile stackoverflow

in case:
<pre>
<code>
set global local_infile = 1
</code>
</pre>
you can now import xml files to the database:
<pre>
<code>
load xml local infile 'Badges.xml' into table badges rows identified by '<row>';
 </code>
  </pre>
the xml files that needs to infile: Badges.xml, Comments.xml, PostHistory.xml, PostLinks.xml Posts.xml, Tags.xml, Users.xml, Votes.xml


Exercise 1: 
```sql
CREATE DEFINER=`root`@`%` PROCEDURE `denormalizeComments`(idPost int(11))
BEGIN
select postId, json_arrayagg(JSON_OBJECT('id', Id, 'postId', PostId, "score", Score, "text", Text, "creationDate", CreationDate, "userId", UserId)) as jsoncomments from comments where postId = idPost;
END
```
Exercise 2:
```sql
DELIMITER $$
CREATE TRIGGER before_comment_update
after insert ON comments
 FOR EACH ROW
 BEGIN
CALL denormalizeComments(new.PostId);
END$$
DELIMITER ;
```
Exercise 3:
```sql
CREATE DEFINER=`root`@`%` PROCEDURE `commentPost`(cId int(11),pId int(11), textM Text, uId int(11))
BEGIN
insert into comments(id, PostId, Score,Text,CreationDate,UserId)values(cId, pId, 0, textM, NOW(), uId);
update posts set commentCount = commentCount+1 where Id = pId;
call denormalizeComments(pId);
END
```
