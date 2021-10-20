### Hi there, I'm Serkan 

![Anurag's GitHub stats](https://github-readme-stats.vercel.app/api?username=anuraghazra&show_icons=true&theme=radical)


- 🔭 I’m currently working on a [VS Code Course][website]!
- 🌱 I’m currently learning everything :)
- 👯 I’m looking to collaborate with other content creators
- 🥅 2022 Goals: Contribute more to Open Source projects
- ⚡ Fun fact: I love to software :)

 /* eslint-disable @typescript-eslint/no-var-requires */
/* eslint-disable @typescript-eslint/camelcase */
const dotenv = require("dotenv");
const fs = require("fs");
const fetch = require("node-fetch");
const querystring = require("querystring");
const http = require("http");
const open = require("open");

if (fs.existsSync(".env")) {
  console.log("Using .env file to supply config environment variables");
  dotenv.config({ path: ".env" });
}

if (process.env["SPOTIFY_REFRESH_TOKEN"]) {
  console.log("Spotify Refresh Token already set, skipping Generation of Refresh Token.");
  process.exit(0);
}

const SERVER_PORT = 3000;
const requiredConfigs = ["SPOTIFY_CLIENT_ID", "SPOTIFY_CLIENT_SECRET"];
const configuration = {};

requiredConfigs.forEach((config) => {
  const envVar = process.env[config];

  if (!envVar) {
    console.error(`Missing config ${envVar}, set as environment variable or add to .env file.`);
    process.exit(1);
  }

  configuration[config] = envVar;
});


const getSpotifyToken = async (authCode) => {
  const encodedCredentials = Buffer.from(`${configuration["SPOTIFY_CLIENT_ID"]}:${configuration["SPOTIFY_CLIENT_SECRET"]}`).toString("base64");

  const getSpotifyTokenUrl = "https://accounts.spotify.com/api/token";
  const body = {
    grant_type: "authorization_code",
    code: authCode,
    redirect_uri: `http://localhost:${SERVER_PORT}/callback`
  };
  const formUrlEncodedBody = querystring.stringify(body);

  const getSpotifyTokenOptions = {
    method: "POST",
    headers: {
      "Content-Type": "application/x-www-form-urlencoded",
      "Authorization": `Basic ${encodedCredentials}`
    },
    body: formUrlEncodedBody,
  };

  const response = await fetch(getSpotifyTokenUrl, getSpotifyTokenOptions);

  if (response.ok && response.body) {
    const data = await response.json();
    return data;
  } else {
    console.error(`Error retrieving access token: ${response.status} - ${response.statusText}`)
  }

  return null;
}

const writeTokenToEnvFile = (refreshToken) => {
  if (fs.existsSync(".env")) {
    fs.appendFile("./.env", `\nSPOTIFY_REFRESH_TOKEN=${refreshToken}`, function (err) {
      if (err) throw err;
      console.log("Refresh Token added to .env file");
    });
  } else {
    fs.writeFile("./.env", `\nSPOTIFY_REFRESH_TOKEN=${refreshToken}`, function (err) {
      if (err) throw err;
      console.log("Refresh Token added to .env file");
    });
  }
};

const server = http.createServer(async function (req, res) {
  if (req.url.startsWith("/callback?code=")) {
    const authCode = req.url.slice("/callback?code=".length);
    const tokenResponse = await getSpotifyToken(authCode);

    if (tokenResponse) {
      writeTokenToEnvFile(tokenResponse.refresh_token);

      res.statusCode = 200;
      res.write(JSON.stringify(tokenResponse, null, 4));
      res.end();
    } else {
      res.end();
    }
  } else {
    res.statusCode = 404;
    res.end();
  }
  server.close();
});

server.listen(SERVER_PORT);

open(`https://accounts.spotify.com/authorize?client_id=${configuration["SPOTIFY_CLIENT_ID"]}&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%3A${SERVER_PORT}%2Fcallback&scope=user-read-playback-state%20user-read-currently-playing`)



<br />

### Languages and Tools:


![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)

![Visual Studio Code](https://img.shields.io/badge/Visual%20Studio%20Code-0078d7.svg?style=for-the-badge&logo=visual-studio-code&logoColor=white)

![HTML5](https://img.shields.io/badge/html5-%23E34F26.svg?style=for-the-badge&logo=html5&logoColor=white)

![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)  


<br />
<br />


### Connect with me:

![Discord](https://img.shields.io/badge/%3CServer%3E-%237289DA.svg?style=for-the-badge&logo=discord&logoColor=white) S3RKAN#4977

![Instagram](https://img.shields.io/badge/<handle>-%23E4405F.svg?style=for-the-badge&logo=Instagram&logoColor=white) 
