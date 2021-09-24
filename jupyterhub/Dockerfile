FROM jupyterhub/jupyterhub:1.4.2

COPY jupyterhub_config.py /etc/jupyterhub/
COPY logo.svg /srv/logo.svg

RUN : \
    && pip install --no-cache --quiet  \
        jupyterhub-idle-culler==1.1 \
        dockerspawner==12.1.0 \
        jupyterhub-nativeauthenticator==0.0.7 \
    && :

# Use --config=<path> or -f=<path>
CMD [ "jupyterhub", "--config=/etc/jupyterhub/jupyterhub_config.py" ]