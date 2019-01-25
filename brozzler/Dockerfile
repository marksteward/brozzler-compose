FROM ubuntu

# See https://github.com/internetarchive/brozzler/blob/master/ansible/roles/brozzler-worker/tasks/main.yml
#  &&  apt-get install -y software-properties-common x11vnc xvfb gnupg apt-transport-https \
#  &&  apt-get install -y dbus wget xfonts-base ttf-mscorefonts-installer fonts-arphic-bkai00mp \
#  &&  apt-get install -y python3-dev python3-gdbm curl git libffi-dev libssl-dev \

# Install Chrome
RUN apt-get update \
    && apt-get -y install curl gnupg \
    && curl -s https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
    && echo 'deb https://dl.google.com/linux/chrome/deb/ stable main' >> /etc/apt/sources.list.d/google-chrome.list \
    && apt-get update \
    && apt-get -y install google-chrome-stable \
    && apt-get purge --auto-remove -y curl gnupg \
    && rm -rf /var/lib/apt/lists/*

# Install fonts, with EULA accepted (slow)
RUN echo ttf-mscorefonts-installer msttcorefonts/accepted-mscorefonts-eula select true | debconf-set-selections
RUN apt-get update \
    && apt-get -y install ttf-mscorefonts-installer \
    && rm -rf /var/lib/apt/lists/*

# Install utils
RUN apt-get update \
    && apt-get -y install curl python3-pip iproute2 runit \
    && rm -rf /var/lib/apt/lists/*

# Install brozzler
ENV LC_ALL=C.UTF-8
RUN pip3 install brozzler[easy]

ADD chrome-headless /usr/local/bin

RUN groupadd -r chrome && useradd -r -g chrome -G audio,video chrome \
    && mkdir -p /home/chrome && chown -R chrome:chrome /home/chrome

EXPOSE 8880 8881

CMD brozzler-easy --chrome-exe=chrome-headless --dashboard-address=''

