# DatabasesAssignment5

we used wget and p7zip in order to get the xml files.

apt-get install p7zip-full. to get p7zip...

wget url to get file...
  
7z e <full zip file name> to extract to current folder.

mysql -u myuser -p --local-infile stackoverflow

in case: set global local_infile = 1

you can now import xml files to the database:

load xml local infile 'Badges.xml' into table badges rows identified by '< row .>';

  
the zip files are: Badges.xml, Comments.xml, PostHistory.xml, PostLinks.xml Posts.xml, Tags.xml, Users.xml, Votes.xml


q1: CREATE DEFINER=`root`@`%` PROCEDURE `denormalizeComments`(idPost int(11))
BEGIN
select json_arrayagg(JSON_OBJECT('id', Id, 'postId', PostId, "score", Score, "text", Text, "creationDate", CreationDate, "userId", UserId)) as jsoncomments from comments where postId = idPost;
END
