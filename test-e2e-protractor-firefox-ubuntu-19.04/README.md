# Test e2e Protractor Firefox Ubuntu 19.04


### To run the `INTERFACE` tests these `VARIABLES` are essential.

| Variables | Example | Description |
| ---- | ---- | ---- |
| `test_front` | *{"scripts": { "test_front": "webdriver-manager update && xvfb-run --server-args='-screen 0 1280x960x24' protractor protractor.conf.js" }}* | You need to configure in your package.json in the scripts section the line that will execute the simplified command.

Below I provided an example of how you could configure your file Dockerfile :

```
FROM ramalhoes/test-e2e-protractor-firefox-ubuntu-19.04:latest

# copy in our source code last, as it changes the most
RUN mkdir -p /opt/app
WORKDIR /opt/app
COPY . /opt/app

RUN npm install -g \
    && npm install -g protractor \
    && npm cache clean --force \
    && webdriver-manager update

# Set the path to the global npm install directory.
ENV PATH /opt/app/node_modules/.bin:$PATH
```

To run the tests inside the *`container`*, just run the following *`command`* on the terminal :

	$ docker run -it -v $(pwd):/opt/app project-name npm run test_front


Remembering that you use *`Firefox`* as your browser the configuration below needs to *`exist`* in your *`protractor.conf.js`* file:

```
capabilities: {
  'browserName': 'firefox'
}
```

# Contributing to this Project

The Docker Hub Images project is maintained by the community. Chief protagonist is @ramalhoes ([Emerson Ramalho](https://github.com/ramalhoes)). Bug reports and pull requests are most welcome.
