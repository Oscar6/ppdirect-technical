# Front End Developer Screening Questions


## 1. Add generic typings to function:\
Add generic typings to the function such that ‘this.request’ returns 
    - a Promise 
    - ‘path’ is a string
    - ‘params’ and ‘headers’ are different objects (provide your own detail) and 
    - ‘headers’ is an optional parameter.

`public static get (path, params, headers) {
 return this.request("GET", path, params, headers);
}`


## 2. Write a typed function
Write a TypeScript function that formats a User object for display. Define the User interface and a function that will take a User object and an optional formatting parameter. The function should return a printable string that has at least 1 alternate format using the format parameter. You may type the User attributes and function parameters as you wish.


## 3. Reduce
What is `result` and what is its type?:\

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


## Responses in `screenerResults.type.ts`