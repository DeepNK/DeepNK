# DeepNK JupyterHub Setup


## Clone our forked [jupyterhub-deploy-docker](https://github.com/DeepNK/jupyterhub-deploy-docker) project

    git clone https://github.com/DeepNK/jupyterhub-deploy-docker

Below instructions assume your working directory is inside your cloned repo.

## Modify `.env` to adapt your needs

If you have used `docker-compose` before then you know that `.env` will control the environment variables
in `docker-compose.yml`

There are two things that you may want to change:

- ### Change `DOCKER_NOTEBOOK_IMAGE` for each user

  After user logged into the jupyterhub, a jupyter notebook webapp container will boot up using `DOCKER_NOTEBOOK_IMAGE` for that user.

  You can pick one from the [jupyter's docker hub](https://hub.docker.com/u/jupyter/) or fork one from their [github](https://github.com/jupyter/docker-stacks).

  Currently, we are using [`jupyter/tensorflow-notebook`](https://hub.docker.com/r/jupyter/tensorflow-notebook/).

- ### Setup GitHub Authentication

  According to the original [`README.md`](https://github.com/DeepNK/jupyterhub-deploy-docker#setup-github-authentication), this part of setup is required. But I think this part is relatively easy. Just click [here](https://github.com/settings/applications/new) to create one.

  After OAuth application creation, you can fill the commented-out variables in `.env` now:

      GITHUB_CLIENT_ID=<github_client_id>
      GITHUB_CLIENT_SECRET=<github_client_secret>
      OAUTH_CALLBACK_URL=https://<myhost.mydomain>/hub/oauth_callback

## Follow the steps to [build the JupyterHub Docker image](https://github.com/DeepNK/jupyterhub-deploy-docker#build-the-jupyterhub-docker-image)

If your server is public accessible, then you may need [Let's encrypt](https://letsencrypt.org/). 

Otherwise, I changed the `Makefile` so you can create self-signed certificates.

    make self-signed-cert

Then, create `userlist` file for members in our organization. For example:

    jtyberg admin

## Build & run the JupyterHub Docker image

    docker-compose up [-d]  # (add `-d` to run in background)

And that's it, for details please refer to original [`README.md`](https://github.com/DeepNK/jupyterhub-deploy-docker).
