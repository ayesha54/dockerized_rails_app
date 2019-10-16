Dockerize New Rails5 Application

Start by setting up the application needed to build the app by using docker-compose. The app will run inside a Docker container containing its dependencies. Defining dependencies is done using a file called Dockerfile.

```docker-compose build```


Connect the database
Replace the contents of config/database.yml with the following:

```ruby
 default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password:
  pool: 5
development:
  <<: *default
  database: myapp_development
test:
  <<: *default
  database: myapp_test 
  ```
  
You can now boot the app with docker-compose up:

``` docker-compose up ```

If all’s well, you should see some PostgreSQL output.

Finally, you need to create the database. In another terminal, run:

```docker-compose run web rake db:create```

View the Rails welcome page!
That’s it. Your app should now be running on port 3001 on your Docker daemon.
On Docker Desktop for Mac and Docker Desktop for Windows, go to http://localhost:3001 on a web browser to see the Rails Welcome.
Now, you can play with the rails application within a container or can develop a whole application by using docker-compose run web as a prefix command.

for eg: ``` docker-compose run web rail c ```

Above command will take you in the rails console
OR you can get into the container itself by running following commands:
``` docker ps # this command will show the running container ```
copy the container id and run following command:
``` docker exec -it <container id> bash ```
This will take you in the container. Now you can work with rails by running any rails commands. for example:
rails console
to exit the container without stopping it :

``` ctrl+q ```

To stop the application, run

``` docker-compose down ```

in your project directory.
Restart the application
To restart the application run docker-compose up in the project directory as you don’t need to rebuild the docker-compose.
Rebuild the application
If you make changes to the Gemfile or the Compose file to try out some different configurations, you need to rebuild. Some changes require only docker-compose up --build, but a full rebuild requires a re-run of docker-compose run web bundle install to sync changes in the Gemfile.lock to the host, followed by docker-compose up --build.
Here is an example of the first case, where a full rebuild is not necessary. Suppose you simply want to change the exposed port on the localhost from 3000 in our first example to 3001. Make the change to the Compose file to expose port 3000 on the container through a new port, 3001, on the host, and save the changes:
ports: - "3001:3000"
Now, rebuild and restart the app with ``` docker-compose up --build ```
Inside the container, your app is running on the same port as before, but the Rails Welcome is now available on your localhost.


