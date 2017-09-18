# kernelci

## Purpose

This repository eases the installation process of KernelCI (frontend + backend)

It uses docker-compose to run the services involded

- kernelci-frontend
- kernelci-backend
- proxy-backend
- redis
- mongo

Additionnal services will be added in a near future.

## Usage

The startup of the application is done in 2 steps:
- start the backend and generate an access token
- use this token to start the frontend (to enable frontend request to smoothly reach the backend)

### Start the backend

This can be done using the following command:

```
docker-compose up proxy-backend
```

This command start the proxy-backend and all the dependent services: backend, redis, mongo.

The API is then up and running and available on port 8000 (temporary port that will be changed soon once the reserve proxy is added in front of the whole application).

We can now use the API to create an admin token from the MASTER_KEY.

```
curl -X POST -H "Content-Type: application/json" -H "Authorization: MASTER_KEY" -d '{"email": "me@gmail.com", "admin": 1}' localhost:8000/token
{"code":201,"result":[{"token":"fd229997-944d-41f3-884f-c4c8b1cd67af"}]}
```

Note: this admin token can then be used to perform any api calls. One of the first action should be to create some non admin tokens in order to perform non admin actions. To keep thing simple, we'll provide the admin token to the frontend (will be changed later on).

### Start the frontend

In this step, we use the token generated above and set it in the *BACKEND_TOKEN* in the *etc/flask_settings*.

We then need to start the frontend with the following command:

```
docker-compose up frontend
```

The web ui is then available on port 80.

![Home](./images/kernelci-home.png)

As the installation has just be done, there are no other resources (jobs, ...) available yet.

![Jobs](./images/kernelci-jobs.png)

## Behind the hood

### Images

Behind the hood, the following temporary images are used:

- [frontend image](https://hub.docker.com/r/lucj/kernelci-frontend)
- [backend image](https://hub.docker.com/r/lucj/kernelci-backend)
- [proxy for backend](https://hub.docker.com/r/lucj/kernelci-proxy-backend)

### Tokens

The tokens are saved in *kernel-ci* mongo database, in the *api-token* collection. The following example shows the content of the token created above.

```
> db["api-token"].find()
{ 
    "_id" : ObjectId("59be6f32618143aa7fc170a2"),
    "username" : null,
    "expired" : false,
    "token" : "fd229997-944d-41f3-884f-c4c8b1cd67af",
    "properties" : [ 1, 1, 1, 1, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0 ],
    "created_on" : ISODate("2017-09-17T12:48:50.604Z"),
    "version" : "1.0",
    "ip_address" : null,
    "email" : "me@gmail.com",
    "expires_on" : null
}
```

## Status

This is a work in progress [WIP], currently not fully functional.

Several features need to be added:
- to be aligned with the official KernelCI
- to improve and simplify the deployment and architecture of the whole application

Among the ongoing changes:

- [ ] Automate the setup (create token from master key, provide token to frontend)
- [ ] Add some tests
- [ ] Check storage part
- [ ] Add api documentation
- [ ] Add elasticsearch and modify backend so log files are sent to ES
- [ ] Add reverse proxy in front of the whole application (routing, TLS, ...)
