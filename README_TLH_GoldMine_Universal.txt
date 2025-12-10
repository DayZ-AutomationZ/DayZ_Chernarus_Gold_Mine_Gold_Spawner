
TLH GOLD MINE – UNIVERSAL INSTALL PACKAGE
=========================================

This package works for:
• Servers with a clean/default init.c
• Servers with an already modified init.c (like Patreon systems, custom functions, etc.)

REQUIREMENTS
------------
• ChernarusPlus map
• Expansion Core installed (for ExpansionGoldNugget / ExpansionGoldBar)

FILES IN THIS PACKAGE
---------------------
1. init_snippet_goldmine.txt
   → Contains all code you must paste into init.c

2. types_goldmine_example.xml
   → Example lifetime entries for Expansion gold items.

3. README (this file)


=========================================
1) INSTALLATION FOR A CLEAN / DEFAULT init.c
=========================================

STEP 1 — Open:
mpmissions/dayzOffline.chernarusplus/init.c

STEP 2 — Find this line near the bottom:
    Mission CreateCustomMission(string path)

STEP 3 — PASTE ALL SNIPPET FUNCTIONS *ABOVE* that line.

Structure should look like:

    ... other functions ...

    // === TLH Goldmine Functions START ===
    void SpawnGoldObject(...) { ... }
    int CountExistingGoldInRadius(...) { ... }
    void SpawnGoldMineLoot() { ... }
    // === TLH Goldmine Functions END ===

    Mission CreateCustomMission(string path)
    {
        return new CustomMission();
    }

STEP 4 — Inside void main(), add this line at the bottom:

    SpawnGoldMineLoot();

Place it ABOVE closing brace of main(), like:

    Print("main finished"); (below whatever is here is perfectly fine.
    SpawnGoldMineLoot();
}

STEP 5 — Add the constant at the top of init.c (if missing):

    static const vector GOLDMINE_CENTER = "8627.1084 0 13301.2100";


=========================================
2) INSTALLATION FOR A CUSTOM / MODIFIED init.c
=========================================

Your init.c may already contain:
• Claim systems
• Spawn systems
• Market systems
• Other custom functions

RULES TO FOLLOW:
----------------
1. GOLD FUNCTIONS MUST BE OUTSIDE ANY CLASS.
   Meaning: paste them in global space, same level as main().

2. GOLD FUNCTIONS MUST BE ABOVE:
       Mission CreateCustomMission(string path)
   (This ensures they compile in time.)

3. You MUST add:
       SpawnGoldMineLoot();
   somewhere inside void main(), after economy/date setup.

4. GOLDMINE_CENTER constant must be placed with other constants at the top.

If you already have other custom functions (like a claim starter kit system or whatever),
DO NOT paste goldmine code inside those classes. Keep them separate.

EXAMPLE LAYOUT FOR CUSTOM SERVERS:
----------------------------------

    // constants
    static const vector GOLDMINE_CENTER = "8627.1084 0 13301.2100";

    // claim system classes...
    // custom systems...

    // goldmine functions
    void SpawnGoldObject(...) { ... }
    int CountExistingGoldInRadius(...) { ... }
    void SpawnGoldMineLoot() { ... }

    // main
    void main()
    {
        ...
        SpawnGoldMineLoot();
    }

    // mission factory
    Mission CreateCustomMission(string path)
    {
        return new CustomMission();
    }


=========================================
3) TYPES.XML SETTINGS (IMPORTANT)
=========================================

Add or edit:

<type name="ExpansionGoldNugget">
    <lifetime>20400</lifetime>   <!-- 6h restart minus 20 min -->
</type>

<type name="ExpansionGoldBar">
    <lifetime>20400</lifetime>
</type>

Lifetime 20400 ensures nuggets always despawn before next restart.


=========================================
4) YOU'RE DONE!
=========================================

Restart your server.
Gold nuggets now appear in a natural “trail cluster” around the mine each restart,
unless players leave too many behind.

If you need help or want an auto-install version for custom mods, just ask.
