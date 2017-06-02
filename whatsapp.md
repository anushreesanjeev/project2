#Api Design for Whatsapp


  ###Registration
* Register with phone number and name 

        ```
            function register(req, res, next) {         
               var data = {
                   name: req.body.name,
                   email: req.body.email,
                   password: req.body..password,
                   phone_number:req.body.phone_number
               };
               db.users.insert(data, function(err, result) {
                   if (err) return next(err);
                   res.send({
                       "status": "sucessfully registered please check your inbox for activation"
                   });
               });
            }

        ```

* After Registration activation code is sent to registered phone number

        ```
        const Nexmo = require('nexmo');
        const nexmo = new Nexmo({
                          apiKey: YOUR_API_KEY,
                          apiSecret: YOUR_API_SECRET
                      });

                    nexmo.message.sendSms(
                        YOUR_VIRTUAL_NUMBER, '15105551234', 'yo',
                              (err, responseData) => {
                        if (err) {
                              console.log(err);
                            } else {
                                  console.dir(responseData);
                              }
                          }
                    );
        ```



### Message 

####one to one message

* select the phone number to which message to send

        ```
        var _id = req.params._id
          db.message.findOne(_id, function(err, result) {
            if (err) return nex(err);
            if (!result) {
                return res.send({
                error: "number not found"
              });
            }
              return res.send(result);
        })

        ```


* create a group with name

            ```
              function group(req, res, next) {         
                  var data = {
                                "groupname": req.body.name,
                                "numbers": [],
                                "messages":[]
                              };
                      db.group.insert(data, function(err, result) {
                        if (err) return next(err);
                            res.send({
                            "status": "group created"
                            });
                      });
              }

            ```

* add users with their phone number
                
                ```
                var _id = req.params.group_id;
                db.group.update(_id, {
                      $addToSet: {
                                  numbers: req.session.user.phone_number
                                }
                  }, function(err, result) {
                if (err) return next(err);
                    db.group.findOne(_id, function(err, number) {
                if (err) return next(err);
                    res.send({
                              status: "updated"
                      });
                  });
                });

                ```