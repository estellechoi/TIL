# Multiple MySQL Versions for Development

Options included below:

- Using Docker docker-compose
- Using Homebrew brew

## Using Docker (recommended)

This gist was originally created for Homebrew before the rise of Docker, yet it may be best to avoid installing mysql via brew any longer. Instead consider adding a barebones docker-compose.yml for each project and run docker-compose up to start each project's mysql service.

### docker-compose.yml

```
version: "3.7"
services:

db:
image: mysql:5.6
restart: always
ports: - "3306:3306"
volumes: - "db_data:/var/lib/mysql"

volumes:
db*data: null
```

Please visit https://hub.docker.com/*/mysql for environment variables that you can optionally set (e.g. username, password, etc.)

To start the your mysql service.

```
docker-compose up
```

To stop your mysql service.

```
docker-compose down # or ctrl-c if service is running in foreground
```

## Using Homebrew

<strong>Disclaimer</strong>: Your milage may vary, I've updated this gist based on comments below but haven't tested it myself since switching to Docker. Please see comments below and/or gist history for additional details.

I was using homebrew version 0.9.5 when writing this gist.

```
brew -v # => Homebrew 0.9.5
```

Install the current version of mysql.

```
# Install current mysql version
brew install mysql

# Start agent for current version of mysql
brew services start mysql
```

Install the older version of mysql.

```
# Install older mysql version
brew install mysql@5.6

# Start agent for older version of mysql
brew services stop mysql
brew services start mysql56
```

Then to switch to the older version.

```
# Unlink current mysql version
brew unlink mysql

# Check older mysql version
ls /usr/local/Cellar/mysql56 # => 5.6.27

# Link the older version
brew switch mysql@5.6 5.6.27
```

And to switch back to the current version.

```
# Unlink older mysql version
brew unlink mysql56

# Check current mysql version
ls /usr/local/Cellar/mysql # => 5.7.10

# Link the current version
brew switch mysql 5.7.10
```

To verify which mysql version you're on at any time.

```
# Check which version of mysql is currently symlinked
ls -l /usr/local/bin/mysql # => /usr/local/bin/mysql@ -> ../Cellar/mysql56/5.6.27/bin/mysql

# Or using the mysql command
mysql --version
```

---

### Reference

- [Multiple MySQL Versions for Development](https://gist.github.com/benlinton/d24471729ed6c2ace731)
