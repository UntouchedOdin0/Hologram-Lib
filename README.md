# Hologram-Lib
[![](https://jitpack.io/v/unldenis/Hologram-Lib.svg)](https://jitpack.io/#unldenis/Hologram-Lib) <br>
Asynchronous, high-performance Minecraft Hologram library for 1.8-1.18 servers.
## Requirements
This library can only be used on spigot servers higher or on version 1.8.8. The plugin <a href="https://www.spigotmc.org/resources/protocollib.1997/">ProtocolLib</a> is required on your server.
## How to use
Add the repository and dependency to your plugin:
Maven
```xml
<repository>
    <id>jitpack.io</id>
    <url>https://jitpack.io</url>
</repository>

<dependency>
    <groupId>com.github.unldenis</groupId>
    <artifactId>Hologram-Lib</artifactId>
    <version>master-SNAPSHOT</version>
</dependency>
```
Gradle
```gradle
allprojects {
    repositories {
        maven { url = 'https://jitpack.io' }
    }
}

dependencies {
    implementation 'com.github.unldenis:Hologram-Lib:master-SNAPSHOT'
}
```
Add ProtocolLib as dependency to your plugin.yml. It could look like this:
```yml
name: Hub
version: 1.0-SNAPSHOT
api-version: "1.13"
depend: [ProtocolLib]
author: unldenis
main: com.github.unldenis.server.hub.ServerHub
```
## Support
The libraries that integrate Hologram-Lib
- <a href="https://github.com/unldenis/NPC-Lib/tree/hologramlib-integration">unldenis/NPC-Lib</a>
## Example usage
```java
public class ExampleHolograms implements Listener {

    private final Plugin plugin;
    private final HologramPool hologramPool;
    /**
     * @param plugin The plugin which uses the lib
     */
    public ExampleHolograms(@NotNull Plugin plugin) {
        this.plugin = plugin;
        this.hologramPool = new HologramPool(plugin, 70);
    }

    /**
     * Appends a new Hologram to the pool.
     *
     * @param location  The location the Hologram will be spawned at
     */
    public void appendHOLO(@NotNull Location location) {
        // building the NPC
        Hologram hologram = Hologram.builder()
                .location(location)
                .addLine("Hello World!")
                .addLine("Using Hologram-Lib")
                .addLine("Hello %%player%%")
                .addLine(new ItemStack(Material.IRON_BLOCK))
                .addPlaceholder("%%player%%", Player::getName)
                .build(hologramPool);

        hologram.getLines().get(3).setAnimation(Animation.CIRCLE);
        // simple changing animating block and text
        timingBlock(hologram);
    }

    private final static Material[] materials = new Material[] { Material.IRON_BLOCK, Material.GOLD_BLOCK, Material.DIAMOND_BLOCK, Material.EMERALD_BLOCK};

    /**
     * Update the block and the first line of text of the hologram
     * @param hologram The hologram to update
     */
    private void timingBlock(Hologram hologram) {
        new BukkitRunnable() {
            int j=1;
            final TextLine firstLine = (TextLine) hologram.getLines().get(0);
            final ItemLine itemLine = (ItemLine) hologram.getLines().get(3);
            @Override
            public void run() {
                if(j==materials.length) j=0;
                firstLine.set(String.valueOf(j));
                itemLine.set(new ItemStack(materials[j++]));
            }
        }
        .runTaskTimer(plugin, 30L, 30L);
    }

    /**
     * Doing something when a Hologram is shown for a certain player.
     * @param event The event instance
     */
    @EventHandler
    public void onHologramShow(PlayerHologramShowEvent event) {
        Hologram holo = event.getHologram();
        Player player = event.getPlayer();
    }

    /**
     * Doing something when a Hologram is hidden for a certain player.
     * @param event The event instance
     */
    @EventHandler
    public void onHologramHide(PlayerHologramHideEvent event) {
        Hologram holo = event.getHologram();
        Player player = event.getPlayer();
    }
}
```
## Preview
https://user-images.githubusercontent.com/80055679/147889286-6d4006a0-677b-4066-a285-08e79d3fad9e.mp4
#### Placeholder Preview
![2022-01-03_22 11 34](https://user-images.githubusercontent.com/80055679/147980899-fa7b8172-b0d8-4ab6-9eab-d33e9323fb63.png)
