This is a fully working example of consuming the runQuery method from the tigergraph.js library, working against TG Cloud.

Store your creadentials in the config.js file:

`const credentials = {
    subDomain: "your-tg-cloud-domain",
    graphDb: "YourGraphName",
    userName: "username",
    secret: "nvg...cvl",
    password: "user-password"
}

module.exports = credentials;`

The query service, including fetching the token and establishing a connection is given in file connectDb.js:

`const tgjs = require("tigergraph.js");
const cred = require("./config.js");

async function queryTgDb(queryName, queryParams) {

    return new Promise(function (resolve, reject) {
        tgjs.getToken(cred.secret, cred.subDomain, 10000, (token) => {
            conn = new tgjs.createTigerGraphConnection(cred.subDomain + ".i.tgcloud.io", cred.graphDb, cred.userName, cred.password, cred.secret, token)                
                conn.runQuery(queryName, queryParams, (data) => {                    
                    resolve(data);
                });
            
        },
        (error) => {
            console.log('Error: Query TG graphDb. Error message: ' + e.message);
            reject(error);
        });
    }); 
}

module.exports = {
    queryTgDb: queryTgDb
};`


In order to consume (query) the above service, use like this:

`const tgdb = require("./connectDb");

async function main() {
    var result = await tgdb.queryTgDb("getPersonByKey", {"keyPerson": "165336"}); //pass over your query name and its params
    console.log('Returned result: ' + JSON.stringify(result));
}

main();`

Calling the runQuery method can be replace by any other method exposed by the tigergraph.js library (createTigerGraphConnection class).
