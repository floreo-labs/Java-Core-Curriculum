# In class exercises
        
### Q1. Write a JSON object to represent the following instance of the `Pizza` class:

```java
Pizza pizza = new Pizza("Mozzarella", "3", true);

// Pizza.java
public class Pizza {
    String cheese;
	int toppingCount;
	boolean fresh;
    
    public Pizza(String cheese, int toppingCount, boolean fresh) {
        this.cheese = cheese;
        this.toppingCount = toppingCount;
        this.fresh = fresh;
    }
}
```

### Q2. Write a JSON object to represent the following instance of this all-new, snazzier `Pizza` class:

```java
Pizza pizza = new Pizza("Provolone", new String[]{"Anchovies", "Olives", "Pineapple"}, false);

// Pizza.java
public class Pizza {
    String cheese;
    String[] toppings;
    boolean fresh;
	
    public Pizza(String cheese, String[] toppings, boolean fresh) {
        this.cheese = cheese;
        this.toppings = toppings;
        this.fresh = fresh;
    }
}
```


### Q3. Write a Java class `Attachment` that represents the following Slack JSON object.

```json
{
            "fallback": "Required plain-text summary of the attachment.",
            "color": "#36a64f",
            "pretext": "Optional text that appears above the attachment block",
            "author_name": "Bobby Tables",
            "author_link": "http://flickr.com/bobby/",
            "author_icon": "http://flickr.com/icons/bobby.jpg",
            "title": "Slack API Documentation",
            "title_link": "https://api.slack.com/",
            "text": "Optional text that appears within the attachment",
            "fields": [
                {
                    "title": "Priority",
                    "value": "High",
                    "short": false
                }
            ],
            "image_url": "http://my-website.com/path/to/image.jpg",
            "thumb_url": "http://example.com/path/to/thumb.png",
            "footer": "Slack API",
            "footer_icon": "https://platform.slack-edge.com/img/default_application_icon.png",
            "ts": 123456789
        }
```

### Q4. Write a Java class `User` that represents a Slack `user` JSON object.

```json
{
    "id": "U023BECGF",
    "name": "bobby",
    "deleted": false,
    "color": "9f69e7",
    "profile": {
        "first_name": "Bobby",
        "last_name": "Tables",
        "real_name": "Bobby Tables",
        "email": "bobby@slack.com",
        "skype": "my-skype-name",
        "phone": "+1 (123) 456 7890",
        "image_24": "https:\/\/...",
        "image_32": "https:\/\/...",
        "image_48": "https:\/\/...",
        "image_72": "https:\/\/...",
        "image_192": "https:\/\/...",
        "image_512": "https:\/\/..."
    },
    "is_admin": true,
    "is_owner": true,
    "is_primary_owner": true,
    "is_restricted": false,
    "is_ultra_restricted": false,
    "has_2fa": false,
    "two_factor_type": "sms",
    "has_files": true
}
```
### Q5. Think of some web services that you like to use (e.g. Facebook, Twitter, Google Maps, Snapchat...). Search Google to find one that has a public API. 
    a. In what ways does the API allow developers to interact with the service? 
    b. If there is a REST API, what endpoints does it offer? 
    c. How does the service format responses (e.g. JSON, XML...)
