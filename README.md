# Weatherapp

_This is a sultion to the Eficode [Weatherapp][1] repo. I used concepts like [containerization](#docker), [cloud infrastructure](#cloud) and automated deployments with [Ansible](#ansible)._

## Prerequisites

- Sign up/in to [OpenWeatherMap][2], and get an API key.
- Create a `.env` file in the root directory with the following information:
  ```
  APPID=<YOUR_OPENWEATHERMAP_API_KEY>
  ```

## Exercises

The app itself is a backend API that pulls weather data from [OpenWeatherMap][2] before serving it to a simple React frontend which displays a current weather icon.

The first is Docker containerization. Then, setting up the containerized app with a cloud service provider. Finally, or automated deployments with Ansible.

### Cloud

Used AWS to deploy the backend and frontend, [AWS link to frontend][7] and [AWS link to backend][8].

### Ansible

The documentation and code be found in the ansible folder of this repo.

### Docker

- Add **Dockerfile**-s in the _frontend_ and the _backend_ directories to run them virtually on any environment having [Docker][3] installed. It should work by saying e.g. `docker build -t weatherapp_backend . && docker run -d -p 9000:9000 --name weatherapp_backend -t weatherapp_backend`. If it doesn't, remember to check your api key first.

The implementation here involves the use of a lightweight **Node-18** and **Node-16** base image, two new [docker-compose][3] files, and a **Makefile** to make things easier.

- `docker-compose.dev-build.yml` is used for setting up NPM modules for both, the backend and frontend, on their own external Docker volumes
- `docker-compose.dev-run.yml` is used for starting up the backend in debug mode, and the frontend in a hot reload development mode.

#### Usage

- Clone the repo and `cd weatherapp/`
- On first run, create the necessary Docker volumes with `make init`
- Run `docker volume ls` and you should see the two newly built volumes `weatherapp_backend_node_modules` and `weatherapp_frontend_node_modules`
- From now on, everytime the `packages.json` file is changed, run `make modules` to do the module installations and persist them on the shared modules volumes
- Otherwise, run `make hotrun` to start both sides of the app in a hot/live-reload mode
- When ready for deployment, run `make containers` or `docker-compose up` to build the images via the Dockerfile-s and run the containers
- Check if everything is OK, then kill the containers and push and/or deploy the `weatherapp_backend` and `weatherapp_frontend` images

[1]: https://github.com/eficode/weatherapp
[2]: http://openweathermap.org
[3]: https://www.docker.com/
[4]: https://docs.docker.com/compose
[5]: https://aws.amazon.com/free
[6]: https://cloud.google.com/free
[7]: http://weatherapp-efcidoe.s3.eu-north-1.amazonaws.com/index.html
[8]: http://44.206.249.75:9000/api/weather
