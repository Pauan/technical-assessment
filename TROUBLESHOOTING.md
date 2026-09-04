When installing the frontend with `npm install` it gave the following error:

```
husky - .git can't be found (see https://typicode.github.io/husky/#/?id=custom-directory)
```

I fixed this by changing the `prepare` hook to `cd .. && husky install frontend/.husky` as recommended by [this answer on StackOverflow](https://stackoverflow.com/a/74297949).

----

I had to run the following PostgreSQL commands to give access to the database:

```
GRANT ALL PRIVILEGES ON DATABASE school_mgmt TO postgres;
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO postgres;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public TO postgres;
```

----

When opening the frontend, it gets stuck on a `Checking permission...` page.

I fixed this by clearing the browser cache.

----

After starting the backend and frontend, I tried logging in, but I got this error:

```
Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at http://localhost:5000/api/v1/auth/login. (Reason: CORS request did not succeed). Status code: (null).
```

This is because the backend is configured to run on port 5007, but the frontend expects the backend to run on port 5000.

I fixed this by changing the `frontend/.env` to use `VITE_API_URL=http://localhost:5007`

----

When logging into the website with the `admin@school-admin.com` credentials, it hangs for a long time and eventually gives this error:

```
error: no pg_hba.conf entry for host "::1", user "postgres", database "school_mgmt", no encryption
```

I fixed this by changing the `pg_hba.conf` file:

```
local all all trust
host  all all ::1/128 trust
```

This is NOT secure and should NEVER be done on production servers, but it's okay for local testing.

----

When running the backend server, it would hang on every request.

I tracked down the problem to the `backend/src/modules/departments/department-module.js` file.

The `initializeHandler` function was throwing this error:

```
ReferenceError: json is not defined
```

This is because the `initializeHandler` is incorrectly assigning to the `json` variable which doesn't exist.

It seems that the `initializeHandler` function is unfinished, since its output is never used anywhere.

I commented out the incorrect code and added a comment.
