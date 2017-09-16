## Install postgis
To install postgis in the first instance, the instructions here were good:
http://www.gis-blog.com/how-to-install-postgis-2-3-on-ubuntu-16-04-lts/

To use postgis with a specifc database, you need to create an extension for that database:
`sudo -u postgres psql -c "CREATE EXTENSION postgis; CREATE EXTENSION postgis_topology;" [database-name]`
