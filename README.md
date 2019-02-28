# DatabasesAssignment5

we used wget and p7zip in order to get the xml files.

mysql -u myuser -p --local-infile stackoverflow

in case: set global local_infile = 1

you can now import xml files to the database:

load xml local infile 'Badges.xml' /*continue for other xml files*/
into table badges
rows identified by '<row>';
