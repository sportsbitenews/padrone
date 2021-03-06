# Padrone

>*Pa-drone*: a master; boss.  
*Pa*: dad.  
*Drone*: A person who does tedious or menial work; a drudge.

HTTP-based master server written in Scala that allows players of various platforms (e.g. Steam/Itch/Oculus) to
play a game with each other.

This is software for game developers that want to setup a master server for their
game such that any player that bought their game, regardless of platform, will be able
to host and join games.

## Features

- Login and verification whether players have bought your game.
- Tracks running hosts.
- Allows players to query the list of hosts.
- Limits one join session per player. Allows the host to kick the player
  once it assumes multiple sessions are in progress on the same account.
- Automatically sorts hosts based on distance (using Ip to Geo location)
- User ids are never handed out, not to clients nor hosts, protecting people's privacy.
- Optionally allows hosts to protect their server with a password

## How does it work?

Player A decides to host a server. The game calls `/register-host` providing login details,
an IP endpoint and a host name. The master server checks the login details and
registers the host in its internal state.

Player B decides to join a game. The game calls `/list-hosts` and Player B chooses
one from this list. The game then calls `/join` with the server id and the master
server returns whether the join succeeded. The client then retrieves a session id
that the game has to send to the host.

Once Player B establishes a connection Player A can call `/player-info`
to verify that Player B indeed called `/join` on the Player A host.

**Please checkout the [wiki](https://github.com/RamjetAnvil/padrone/wiki) for further details.**

## How to install

- Clone the repo and go the `server` folder and build a jar with `sbt assembly`.
- Copy the `example-application.conf` and fill in your Steam/Itch/Oculus server keys.
- Put the `application.conf` in the same directory as the jar or put it in the
`src/main/resources` folder before creating an assembly.
- Start the server with `java -jar server.jar`
- Make sure to use another webserver to provide HTTPS and redirect the requests
to this server.

## The client

The `csharp_client` folder contains a Unity-based client.

## To be done

- Integrate Steam friends list to allow players to join a friend's game.
- Pluggable facilities for match-making

## License

Distributed under the MIT License, see LICENSE.txt for the full text.
