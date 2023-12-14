# SD500-Final-Project-PetFinder-API
Create an application to browse the [Petfinder API](https://www.petfinder.com/developers/v2/docs/).

Head to the [Petfinder API](https://www.petfinder.com/developers/v2/docs/) website and create a free account. Go to `Developer Settings` and retrieve your `API Key` and `Secret Key`. 
  
When your application bootstrap, send a `POST` request to `https://api.petfinder.com/v2/oauth2/token` and include the following JSON in the request body:
```javascript
{
   "grant_type": "client_credentials",
   "client_id": "YOUR_API_KEY_HERE",
   "client_secret": "YOUR_SECRET_KEY_HERE"
}
```
The response returns the following body: `{ "token_type": string, "expires_in": number, "access_token": string }`.
* You are allowed to send 50 requests per second, with a total of 1000 requests per day. Keep an eye on your daily usage in the `Developer Settings` page.
* The `access_token` expires in 1 hour.
* You will need to include the `access_token` in all future requests headers as follows: `{ 'Authorization': 'Bearer access_token' }`
* Make sure [jwt-decode](https://github.com/auth0/jwt-decode#readme) package is installed (`npm i jwt-decode`). Use the package to decode the `access_token` and read its content.
```typescript
import { jwtDecode } from "jwt-decode";
type JWTType = { exp: number };

const token = "eyJhsw5c...";
const decoded = jwtDecode<JWTType>(token); // Returns { exp: number }
```
* The `access_token` contains an `exp` property which is the timestamp of expiration, you may compare it with `Date.now()` to know if the `access_token` has expired or not.
* Make sure you have [node-localstorage](https://github.com/lmaccherone/node-localstorage) package installed (`npm i node-localstorage` and `npm i @types/node-localstorage`), and persist both `access_token` and `exp` values on the hard-disk, to be retrieved when the application reloads.
* Your application must re-fetch a new `access_token` only if the previous one is expired.
* Make sure you have [prompts](https://github.com/terkelg/prompts#readme) package installed (`npm i prompts` and `npm i @types/prompts`) and use the [Select](https://github.com/terkelg/prompts?tab=readme-ov-file#selectmessage-choices-initial-hint-warn) feature to ask the following list of questions:
    1. Animal `name`, value type is `string` - optional
    2. Animal `type`, value type is `select` from two choices `Dog` or `Cat` - required
    3. Animal `gender`, value type is `select` from the two choices `Male` or `Female` - required
* Once you collect the answers of the above questions, send a request to `GET https://api.petfinder.com/v2/animals` and pass the choices as query string, for example: `?name=Theo&type=Dog&gender=Male`, or `?type=Dog&gender=Male` if the name was null.
* Display the pets `name` as choices for a new `select` prompt, where the pet `name` is used as `title` and `id` is used as a `value` for every `choice`. 
* When the user picks a pet, fetch the pet details by `id` from the following API endpoint: `GET https://api.petfinder.com/v2/animals/{id}` and display the pet details: `name`, `breed`, `size`, `age`, `color`, `status`.
* Redirect the prompt back to the 3-questions prompt.
  
### Bonus Requirement (3 extra points)
Integrate a list of bookmarks, as array of pet name and id, add a prompt to add or remove a pet from the bookmark list after the pet details are displayed, display the list of bookmarks after you add or remove the pet from the list and then take the user to the 3-questions prompt. The bookmark list must also be persisted on the hard-disk and retrieved when the application bootstrap. 

### Application specifications
* Use Object-Oriented programming when possible.
* Divide your application into separate modules.
* All Response must be typed, and your application must not have objects with type `any`.
* A daily push is required to track your code progress and measure your performance. 

## Final Evaluation 
The submission deadline is on Tuesday at 9:00 PM. I will meet with you on Wednesday and Thursday as per the schedule and evaluate the final project code.  

Good luck, and happy coding!

_Code Honor Submission Policy: Remember to respect the code honor submission policy. All written code must be original. Presenting any code as one’s own work when it came from another source is plagiarism, which includes any matching patterns and code snippets, and will affect your grade. The use of AI is not permitted in this assignment. For more details, check the full course policies in the syllabus._
