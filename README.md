passport-parse
==============

A passport strategy for those of you out there using Parse.com and nodejs (passport) for authentication :)

See [Authenticate.me Node server](https://github.com/malikov/Authenticate.me-Node-Server) for an example of how to use the strategy

## how to use
clone this repo and place it in your node_modules folders
				
	git clone https://github.com/malikov/passport-parse

or Install using npm

	npm install passport-parse

Include both passport and passport-parse

	var passport = require('passport');
	var ParseStrategy = require('passport-parse');


Create a new parseStrategy configuration

	var parseStrategy = new ParseStrategy({
		parseAppId,
		parseJavascriptKey
	});

or you could just load a parse client and pass it in the strategy
	
	var parse = require('parse').Parse;
	parse.initialize(config.parse.appId,config.parse.jsKey);

	var parseStrategy = new ParseStrategy({parseClient: parse});


Then add the strategy to passport

	passport.use(parseStrategy);

And authenticate the user like so : 

	passport.authenticate('parse',function(err, user, info) {
	    if (err) {
	    	return res.status(400).json({payload : {error: info}, message : info.message});
	 	}

		if (!user) { 
		   	return res.status(400).json({payload : {error: info}, message : info.message});
		}

		req.logIn(user, function(err) {
			   	if (err) {
			   		return res.status(400).json({payload : {error: err}, message : info.message});
				}
			 		
				return res.json({
			   		payload : req.user,
			   		message : "Authentication successfull"
			   	});
			});
		})(req,res);

