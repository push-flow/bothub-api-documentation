# Bothub API Documentation



### Authentication

First you need to authenticate on Bothub API, to do so you need to make a request to the following endpoint:

- URL: http://api.bothub.it/auth

- Method: GET

- Return example:

  ```json
  {
      "uuid": "bf925d0b24f34c0988f5070ec3cfe12g"
  }
  ```

### Training a new Bot

Now you need to train your intents using an existing open source tool https://rasahq.github.io/rasa-nlu-trainer/, create intents and sentences according to your needs. After that click on "Download";

Once you have the file you need to transform it content into Bothub format, which basically needs the language and a unique slug to identify your bot. Add it to Bothub by calling:

- URL: http://api.bothub.it/train-bot

- Method: POST

- Headers:

  ```http
  Content-Type: application/json
  Authorization: Bearer 996e83d3e3fb4fdd9bcdf7a870e7c8c6
  ```

- Input example:

  ```json
  {
    "language": "en",
    "slug": "basic",
    "data": {
      "rasa_nlu_data": {
        "common_examples": [{
          "text": "hey",
          "intent": "greet",
          "entities": []
        },
        {
          "text": "show me a mexican place in the centre",
          "intent": "restaurant_search",
          "entities": [{
          "start": 31,
          "end": 37,
          "value": "centre",
            "entity": "location"
          },
          {
            "start": 10,
            "end": 17,
            "value": "mexican",
            "entity": "cuisine"
          }]
        }]
      }
    }
  }
  ```

- Return example:

  ```json
  {
    "owner": "996e83d3e3fb4fdd9bcdf7a870e7c8c6",
    "slug": "basic",
    "uuid": "0ebcce28-1036-4073-9928-7f6a630f10c0"
  }
  ```

### Using Bothub to predict

Now your bot is ready to predict intents from messages by using `bots` endpoint:

- URL: http://api.bothub.it/bots?uuid=0ebcce28-1036-4073-9928-7f6a630f10c0&msg=hey

- Parameters: 

  - uuid: Bot identifier collected from the previous step;
  - msg: Message you want to predict

- Method: GET

- Headers:

  ```http
  Content-Type: application/json
  Authorization: Bearer 996e83d3e3fb4fdd9bcdf7a870e7c8c6
  ```

- Return example:

  ```json
  {
      "bot_uuid": "0ebcce28-1036-4073-9928-7f6a630f10c0",
      "answer": {
          "text": "hey",
          "entities": [],
          "intent_ranking": [
              {
                  "confidence": 0.5580678887164175,
                  "name": "greet"
              },
              {
                  "confidence": 0.33171625012713263,
                  "name": "affirm"
              },
              {
                  "confidence": 0.08520317310657238,
                  "name": "goodbye"
              },
              {
                  "confidence": 0.025012688049877527,
                  "name": "restaurant_search"
              }
          ],
          "intent": {
              "confidence": 0.5580678887164175,
              "name": "greet"
          }
      }
  }
  ```
