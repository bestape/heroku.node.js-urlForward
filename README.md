# smart url forward for heroku
## why
To forward from one domain to the next: 
* `<APP_NAME>.herokuapp.com/<URN_PATH_AFTER_FIRST_/>` to `<DESTINATION_URL>`.

For example:
* [code.bestape.net/lineNbr](http://code.bestape.net/lineNbr) forwards to https://raw.githubusercontent.com/bestape/.0.sh-project/master/lineNbr.js via this webapp; and
* [code.bestape.net](http://code.bestape.net) provides a json object that lists the [urn](https://en.wikipedia.org/wiki/Uniform_resource_name) paths available at code.bestape.net. 

## about
This webapp is designed to use the benifits of: (1) a personally controlled server configured with [.0.sh](http://0.sh.bestape.net); and (2) a [heroku server](https://en.wikipedia.org/wiki/Heroku). It follows Unix philosophy's [Rule of Separation](https://en.wikipedia.org/wiki/Unix_philosophy#Eric_Raymond.E2.80.99s_17_Unix_Rules). 

The personally controlled server's primary job is to publish the `content/data.json` file for the heroku server. `content/data.json` is what the user manually edits on a going forward basis to modify the webapp's setting. This system is the front-end interface. It provides the webapp's data.

The heroku server's primary job is to be the system that hosts the back-end engine. It provides the webapp's logic processing of data into information. It is the server that distributes and communicates the published content to the public.

To turn the heroku server on/off or change the paths it redirects to, the desired effects should be written in the personally controlled server's `content/data.json` files and passed (cf. published) to the heroku server. The personally controlled server does not need to uninterruptedly serve the `content/data.json` file to the heroku server. However, since heroku uses an entirely [ephemeral filesystem](https://devcenter.heroku.com/articles/dynos#ephemeral-filesystem), the personally controlled server must serve the `content/data.json` file to the heroku server whenever the heroku server wakes from hibernation or otherwise erases its dynamic storage.

##setup
On a personally controlled server (e.g. an [EC2](https://aws.amazon.com/ec2) instance), run the following commands.

###install urlForward's dependencies
* `git clone http://heroku.node.js-urlForward.bestape.net urlForward`;
* `cd urlForward`;
* `npm init`; and
* `npm install`.

###customize urlForward
* In the `posit.json` file, replace the `"<CONTENT_DESTINATION_URL>"` value within the `"uri"` object, that is within the `"content"` object, to the personally controlled server's url address;
* In the `content/data.json` file, replace the `["<URN_PATH_AFTER_FIRST_/>", "<DESTINATION_URL>"]` values in the `"paths"` array's 0 key with specific and applied urn and url information; and
 * Add as many additional vales to in the array as needed; and
 * each value in the `"paths"` array must be a paired array with key 0 a `"<URN_PATH_AFTER_FIRST_/>"` value and key 1 a `"<DESTINATION_URL>"` value.
* In the `content/posit.json` file, switch the `"port"` object to the port the personally controlled server will host from.

###if no prior heroku work exists
* install [heroku](https://toolbelt.heroku.com);
* `heroku login`; and
* `heroku apps:create <APP_NAME>`.

###else if prior heroku work exists
* `heroku keys:add`; and
* `git remote add <APP_HEROKU_GIT_URL>`. 

###heroku deploy
* `git commit -am "customized prototype git for specific use"`; and
* `git push heroku master`.

###serve content to heroku deploy
* `./content/posit.js`.

## license
[heroku.node.js-urlForward](http://heroku.node.js-urlForward.bestape.net) is released under the [ISC](http://www.isc.org/downloads/software-support-policy/isc-license) license.

Copyright &copy; 2013-2014 by [bestape](mailto:heroku.node.js-urlForward@bestape.net) 

Permission to use, copy, modify, and/or distribute this software for any purpose with or without fee is hereby granted, provided that the above copyright notice and this permission notice appear in all copies.

THE SOFTWARE IS PROVIDED "AS IS" AND THE AUTHOR DISCLAIMS ALL WARRANTIES WITH REGARD TO THIS SOFTWARE INCLUDING ALL IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS. IN NO EVENT SHALL THE AUTHOR BE LIABLE FOR ANY SPECIAL, DIRECT, INDIRECT, OR CONSEQUENTIAL DAMAGES OR ANY DAMAGES WHATSOEVER RESULTING FROM LOSS OF USE, DATA OR PROFITS, WHETHER IN AN ACTION OF CONTRACT, NEGLIGENCE OR OTHER TORTIOUS ACTION, ARISING OUT OF OR IN CONNECTION WITH THE USE OR PERFORMANCE OF THIS SOFTWARE.

## notes
###tinkerable
The personally controlled server's git can be used as a [revision control](https://en.wikipedia.org/wiki/Source_code_management) hub for the webapp's front-end and back-end content: 
* Run `./publicPrivateSwitch.js` to change between the generic and specific versions of the webapp; 
* use the `urlForward` folder for the generic versions of the webapp that can be used as the prototype for various circumstances; and
 * The git commits in this folder are theoretical developer versions of the webapp.
* create a clone of the `urlForward` folder with `cp -r urlForward urlForward.publish` and use the `urlForward.publish` folder for the specific versions of the webapp.
 * The git commits in this folder are applied instance versions of the webapp that are runnable; and
 * `git push heroku master --force` has to be used if the `urlForward` prototype used by the heroku server is different from the `urlForward` prototype used by `urlForward.publish`. 

###tensor variables & declarative sentence functions
The script uses an experimental organizational system. 
