### Dradis Docker Container

A [Docker](https://www.docker.com/) image with [Dradis-CE](http://dradisframework.org/).

This is a fork of [zuazo/dradis-docker](https://github.com/zuazo/dradis-docker), updating it to use the active dradis-ce repo.

This also has an `arch`-branch for an Arch Linux based image, which is more of an artifact of trying to fix an issue building on a grsecurity host (still doesn't work) than a recommended image as there's currently no reasonably minimal arch base image available, making it quite large.

### Supported Tags and Respective `Dockerfile` Links

* `latest` ([*/Dockerfile*](https://github.com/zuazo/dradis-docker/tree/master/Dockerfile))

#### What Is Dradis?

From [its own website](http://dradisframework.org/):

*The Dradis Framework is an open-source collaboration and reporting platform for IT security experts.*

*Dradis is a self-contained web application that provides a centralized repository of information to keep track of everything that has been done so far, and what is still ahead.*

#### How to Use This Image

##### Build

    $ git clone https://github.com/evait-security/dradis-docker-ce.git
    $ cd dradis-docker-ce
    $ docker build -t evait/dradis-ce .

##### Create a Directory to Store the Database Data

    $ mkdir -p dbdata/

##### Create a Directory to be able to  add / modify templates

    $ mkdir -p templates/

##### Setup a Redis instance

    $ docker run --name dradis-redis -d redis:alpine

##### Run Dradis

You need to set the `/dbdata` and `/templates` volume path and link the Redis container:

    $ docker run \
        --publish 3000:3000 \
        --volume "$(pwd)/dbdata:/dbdata" \
        --link dradis-redis:redis \
        --volume "$(pwd)/templates:/opt/dradis-ce/templates" \
      evait/dradis-ce

You can now open [http://127.0.0.1:3000/](http://127.0.0.1:3000/) to access Dradis.

#### Exposed TCP/IP Ports

* `3000`: Dradis application HTTP port.

#### Environment Variables Used at Runtime

* `SECRET_KEY_BASE`: Randomized string which is used to verify the integrity of signed cookies (randomly generated by default). See [here](http://edgeguides.rubyonrails.org/upgrading_ruby_on_rails.html#config-secrets-yml).

You can change them using `docker run -e [...]` or in your *Dockerfile*, using the `ENV` instruction.

#### Read-only Environment Variables Used at Build Time

* `RAILS_ENV`: Rails environment (`production`).

The docker working directory is set to the main Dradis directory (`/opt/dradis-ce`).

### License and Author

|                      |                                          |
|:---------------------|:-----------------------------------------|
| **Author:**          | [Xabier de Zuazo](https://github.com/zuazo) (xabier@zuazo.org)
| **Copyright:**       | Copyright (c) 2016
| **License:**         | Apache License, Version 2.0

```
Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
