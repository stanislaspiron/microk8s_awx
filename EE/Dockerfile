FROM quay.io/ansible/awx-ee:0.1.1

USER root

ADD requirements.yml /tmp/requirements.yml
ADD requirements.txt /tmp/requirements.txt

RUN ansible-galaxy collection install -r /tmp/requirements.yml --collections-path /usr/share/ansible/collections
RUN pip --disable-pip-version-check install -r /tmp/requirements.txt  # for any Python modules the collections didn't pull in

USER 1000
