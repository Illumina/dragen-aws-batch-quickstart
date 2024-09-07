# DRAGEN Quickstart Docker image generator --
FROM public.ecr.aws/docker/library/oraclelinux:8
RUN rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm

RUN /usr/bin/crb enable

# Install Basic packages needed for Dragen
RUN dnf -y install \
  perl \
  sos \
  coreutils \
  gdb \
  time \
  systemd-libs \
  bzip2-libs \
  R \
  ca-certificates \
  ipmitool \
  smartmontools \
  rsync \
  tree && \
  yum clean all && \
  mkdir -m777 -p /var/log/dragen /var/run/dragen


#########################################################
# Now install the Edico WFMS "Wrapper" functions

# Install necessary standard packages
# Note: 'easy_install' used to workaround docker/kernel issue (https://github.com/moby/moby/issues/12327)
RUN dnf install python3.12 python3.12-pip && \
    dnf clean all

RUN python3 -m pip install --upgrade pip && \
    python3 -m pip install future six requests boto3

# Install d_haul and dragen_job_execute wrapper functions and associated packages
RUN mkdir -p /root/quickstart/scheduler && \
    touch /root/quickstart/scheduler/__init__.py
COPY src/d_haul src/dragen_qs.py /root/quickstart/
COPY src/scheduler/aws_utils.py src/scheduler/logger.py src/scheduler/scheduler_utils.py  \
    /root/quickstart/scheduler/

# Landing directory should be where the run script is located
WORKDIR "/root/quickstart/"

# Debug print of container's directories
RUN tree /root/quickstart/

# Default behaviour. Over-ride with --entrypoint on docker run cmd line
ENTRYPOINT ["python3","/root/quickstart/dragen_qs.py"]
CMD []
