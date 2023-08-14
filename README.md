## Front End Developer Screening Questions


## 1. Add generic typings to function

```
Add generic typings to the function such that ‘this.request’ returns 
    - a Promise 
    - ‘path’ is a string
    - ‘params’ and ‘headers’ are different objects (provide your own detail) and 
    - ‘headers’ is an optional parameter.

public static get (path, params, headers) {
  return this.request("GET", path, params, headers);
}
```

## Response:

```
type RequestOptions = {
  method: string;
  path: string;
  params?: Record<string, any>;
  headers?: Record<string, string>;
};

class GenClass {
    // Request
    public static async request<T>(method: string, path: string, params?: Record<string, any>, headers?: Record<string, string>): Promise<T> {
        const queryParams = new URLSearchParams(params).toString();
        const fullPath = queryParams ? `${path}?${queryParams}` : path;

        const response = await fetch(fullPath, {
            method,
            headers,
        });

        const data = await response.json();
        console.log('Response: ', data);
        return data as T;
    }

    // GET request
    public static async get<T>(path: string, params?: Record<string, any>, headers?: Record<string, string>): Promise<T> {
        const response = await this.request("GET", path, params, headers);
        console.log('Response: ', response);
        return response;
    }
}

// Type assertions to verify typings
type VerifyRequestPromise = ReturnType<GenClass['request']>;
type VerifyPath = Parameters<GenClass['request']>[1];
type VerifyParams = Parameters<GenClass['request']>[2];
type VerifyHeaders = Parameters<GenClass['request']>[3];
```


## 2. Write a typed function
Write a TypeScript function that formats a User object for display. Define the User interface and a function that will take a User object and an optional formatting parameter. The function should return a printable string that has at least 1 alternate format using the format parameter. You may type the User attributes and function parameters as you wish.

// Response:

```
interface User {
  firstName: string;
  lastName: string;
  petName: string;
  petAge: number;
  email: string;
}

type FormattingOptions = 'default' | 'uppercase' | 'full';

function formatUser(user: User, format: FormattingOptions = 'default'): string {
  let formattedString: string;

  switch (format) {
    case 'uppercase':
      formattedString = `Name: ${user.firstName.toUpperCase()} ${user.lastName.toUpperCase()},\nPet: ${user.petName.toUpperCase()},\nPet Age: ${user.petAge},\nEmail: ${user.email.toUpperCase()}`;
      break;
    case 'full':
      formattedString = `Full Details:\nName: ${user.firstName} ${user.lastName}\nPet: ${user.petName}\nPet Age: ${user.petAge}\nEmail: ${user.email}`;
      break;
    default:
      formattedString = `Name: ${user.firstName} ${user.lastName},\nPet: ${user.petName},\nPet Age: ${user.petAge},\nEmail: ${user.email}`;
  }

  return formattedString;
}

const user: User = {
  firstName: 'Oscar',
  lastName: 'Miranda',
  petName: 'Olive',
  petAge: 7,
  email: 'email@gmail.com'
};

// Default format
console.log(formatUser(user));

// Uppercase format
console.log(formatUser(user, 'uppercase'));

// Full format
console.log(formatUser(user, 'full'));
```


## 3. Reduce
What is `result` and what is its type?

```
const t = (medication:IMedication): IPrescription => {
     const map: { [key: string]: string } = {
         medication_id: 'prescription_id',
         display_name: 'drug_name',
         directions: 'instructions',
         notes: 'instructions',
         quantity: 'dispense',
     };
     return Object.keys(medication).reduce((carry: Partial<IPrescription>, k:string) => {
         return {
             ...carry,
             [( map[k] !== undefined ? map[k] : k)]: medication[k],
         }
     },{});
 }
 const medication: IMedication = {
     medication_id: 12345,
     display_name: "Lipitor",
     strength: "20 mg",
     route: "by mouth",
     directions: "Take 1 tablet daily",
     quantity: 30,
 };
 const result = t(medication);
```

## Response:

```
`result` is the variable which returns the value of the function `t`
The type of `result` is `Partial<IPrescription>` which contains some of the properties of `IPrescription` below:
     {
         prescription_id: 12345,
         drug_name: "Lipitor",
         instructions: "Take 1 tablet daily",
         dispense: 30
     }
```