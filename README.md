This is the Official API for Volcano Core (https://builtbybit.com/resources/volcano-ranks-punishments-disguise.29107/)
To run this API, it must be within Bukkit, BungeeCord or Velocity. and the Volcano plugin for the said platform must be enabled and running.

# Usage
all related API usages will be within the CoreAPI class

```java
CoreAPI.getInstance();
```

# Getting an online Profile

```java
Profile profile = CoreAPI.getInstance().getProfileManager().getByUUID(uuid); // using uuids
Profile profile = CoreAPI.getInstance().getProfileManager().getByName(name); // using player names
```

# Getting a Profile for on offline player

```java
Profile profile = CoreAPI.getInstance().getProfileManager().findById(uuid);
```

# Punishments
Sadly the Punishment API is only available on Bukkit only.

## Adding a punishment

```java
Punishment punishment = new Punishment(UUID.randomUUID());
punishment.setPlayer(profile.getUuid()); // the uuid of the player
punishment.setType(PunishmentType.BAN); // Bans
punishment.setIssuer(uuid); // null for Console
punishment.setReason("Any reason here");
punishment.setDuration(-1)); // -1 for permenant (in milliseconds)

boolean silent = true;

CoreAPI.getInstance().getPunishmentManager().handlePunishment(punishment, silent);
```

## Removing a punishment

```java
CoreAPI.getInstance().getPunishmentManager().handleRemovePunishments(Profile profile, Profile remover, String reason, Punishment punishment, boolean silent);
```

# Ranks

## Getting a rank

```java
Rank rank = CoreAPI.getInstance().getRankManager().getByUUID(uuid); // using uuids
Rank rank = CoreAPI.getInstance().getRankManager().getByName(name); // using names

Rank rank = CoreAPI.getInstance().getRankManager().getDefault(); // the default rank
```

## Deleting a rank

```java
CoreAPI.getInstance().getRankManager().delete(rank);
```

# Grants

## Adding a grant

```java
CoreAPI.getInstance().getGrantManager().grant(Profile profile, UUID issuer, ServerScope scope, Rank rank, long durationMillis, String reason, Server currentServer);
```

## Removing a grant

```java
CoreAPI.getInstance().getGrantManager().removeGrant(Profile profile, UUID remover, Grant grant);
```

## Event

```java
@EventHandler
public void onGrant(PlayerGrantEvent event) {
  Grant grant = event.getGrant();
}
```

# Disguises
Disguises API is only available in Bukkit

```java
CoreAPI.getInstance().getDisguiseManager().disguise(Player player, String user, Rank rank);
```

## Undisguising

```java
CoreAPI.getInstance().getDisguiseManager().undisguise(Player player, Profile profiler);
```

There are way more you can do with the API that I have not covered, you can look at CoreAPI.getInstance() and look through the managers 
all the methods are pretty straight-forward.
