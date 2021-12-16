FROM jupyterhub/jupyterhub:2.0.0

COPY jupyterhub_config.py /etc/jupyterhub/
COPY logo.svg /srv/logo.svg

RUN : \
    && pip install --no-cache --quiet \
        jupyterhub-idle-culler==1.2.1 \
        dockerspawner==12.1.0 \
        jupyterhub-nativeauthenticator==1.0.5 \
    && :

# Use --config=<path> or -f=<path>
CMD [ "jupyterhub", "--config=/etc/jupyterhub/jupyterhub_config.py" ]