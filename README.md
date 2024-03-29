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
Disguise API is only available in Bukkit

```java
CoreAPI.getInstance().getDisguiseManager().disguise(Player player, String user, Rank rank);
```

## Undisguising

```java
CoreAPI.getInstance().getDisguiseManager().undisguise(Player player, Profile profiler);
```

# Settings

## Creating your custom setting
setting's values can be anything

```java
public class ExampleSettingsItem extends AbstractSettingsItem {

    @Override
    public ItemStack getItemStack(Player player) {
        ItemStack itemStack = new ItemStack(Material.DIAMOND_SWORD);
        ItemMeta meta = itemStack.getItemMeta();

        boolean enabled = CoreAPI.getInstance().getProfileManager().getByUUID(player.getUniqueId())
                .getSettings().getSettingOrDefault("test", true); // gets the setting and if it isn't available returns the value given

        meta.setDisplayName(ChatColor.AQUA + "Test Setting");

        String on = ChatColor.GREEN + "■";
        String off = ChatColor.GRAY + "■";

        meta.setLore(Arrays.asList(
                ChatColor.GRAY + "Text here",
                "",
                (enabled ? on : off) + ChatColor.YELLOW + " Enabled",
                (enabled ? off : on) + ChatColor.YELLOW + " Disabled"
        ));

        itemStack.setItemMeta(meta);

        return itemStack;
    }

    @Override
    public int getSlot(Player player) {
        return 5; // the slot where the item will be placed in the settings menu
    }

    @Override
    public void onClick(Player player, ClickType clickType) {
        Profile profile = CoreAPI.getInstance().getProfileManager().getByUUID(player.getUniqueId());
        boolean setting = profile.getSettings().getSettingOrDefault("test", true); // getting the value for 'test' and if it doesnt exist we return the value given in the second parameter

        profile.getSettings().setSetting("test", !setting); // toggling the 'test' setting
        CoreAPI.getInstance().getProfileManager().saveAsync(profile); // save the profile after the changes
    }

    @Override
    public boolean updateAfterClick(Player player) {
        return true; // if true the menu is updated after click
    }
}
```

## Registering the SettingsItem

```java
CoreAPI.getInstance().getSettingsItems().add(new ExampleSettingsItem());
```

# ServerManager
The server-manager allows you to do much stuff like retrieving information about a specific server (online players, max players, status, tps, and more!)

## Fetching a specific server using it's UUID

```java
Server server = CoreAPI.getInstance().getServerManager().fetchServer(serverUUID); // this gets the server from Redis
Server server = CoreAPI.getInstance().getServerManager().getCachedServer(serverUUID); // this gets the server from local cache (bukkit only)
```

# RebootManager
only available on Bukkit

```java
long getTimeLeftBeforeReboot(); // in milliseconds

void startShutdown();

void pauseReboot();

void resumeReboot();

boolean isRebootPaused();
```

There is way more you can do with the API that I have not covered, you can look at CoreAPI.getInstance() and look through the managers 
All the methods are straightforward.

For any help join the discord https://discord.gg/tqJwmJZDbe
