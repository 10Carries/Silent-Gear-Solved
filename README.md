# Silent Gear Solved

A material and alloy ranking calculator for the Minecraft Silent Gear mod. Drop in your material export, tell it what stat you want to optimize, and it ranks every material and forged alloy from best to worst so you don't have to spend hours experimenting in game.

Access the website at https://10carries.github.io/Silent-Gear-Solved/

## What it does

Silent Gear has a lot of materials that can be forged together into alloys with different stats. Figuring out the single best alloy for, say, maximum durability is not something you want to work out by hand. This tool does it for you:

* **Single Materials** ranks every individual material for a part type (Main, Grip, Coating, etc.) by whatever stats you select. (Honestly not sure why I still have this. I added it at the start in case someone doesn't want to deal with the hassle of making like a metal alloyer or something. But then why are you here? I'll keep it for now I guess but I might remove it in the future.)
* **Forged Alloys** searches through combinations of 2 or more materials melted together in an Alloy Forge (also called Metal Alloyer in some versions of the mod), Super Mixer, Recrystalizer, or Refabricator, and ranks the resulting alloys.

## Getting your material data

The calculator needs a material export from your actual modpack, since material stats vary between packs and mod versions.

1. In any world with Silent Gear installed, run:
   ```
   /sgear mats dump
   ```
2. This produces a `.tsv` (tab separated values) file. Find it and note where it saved.
3. On the site, drag that file onto the upload box (or click it to browse for the file).

If the file doesn't look like a valid export, you'll get an error telling you what's missing usually because it's the wrong file or was edited.

## Step by step

### 1. Load your export

Upload your `.tsv` file as described above. Once it loads, you'll see a summary of how many materials were found.

### 2. Pick what you value

Check off whichever stats matter to you Durability, Attack Damage, Armor, Enchantment Value, and so on. You can select as many as you like. Materials and alloys are ranked by their *average rank* across every stat you selected, so picking several stats can find well rounded options, while picking just one finds the single best specialist. (I have some ideas in mind for future versions of this website where you can sort by something like attack damage * attack speed to get dps or something)

Use **Select all** / **Clear** to quickly toggle and/or reset your picks.

### 3. Choose a tab

**Single Materials** pick a part type from the dropdown (Main, Binding, Grip, etc.) and see every material of that type ranked. Most things just use main but if you are making a bow for example, you might want another option for the cord.

**Forged Alloys** Select a few options before the code tries making all alloys:

* **Silent Gear version** pick the version of Silent Gear that matches your modpack. This affects how many slots each machine has and whether the Super Mixer exists at all. (I'm pretty sure I labeled all the versions correctly but I didn't manually test all minecraft versions with silent gear versions so if there are any inaccuracies please let me know.)
* **Machines available** Select any alloying stations that you don't have access to (or that are disabled in your pack). Each machine accepts different material categories and slot counts.
* **Rank & display by role** alloys have different stats and traits depending on which role they'll end up filling (e.g. a Main head vs. a Tip Upgrade). Pick the role you're actually building for.
* **Max alloy size** how many materials can go into one alloy. Bigger alloys mean vastly more combinations to check, so this has a big effect on search time. If you have a couple minutes to spare, I recommend just setting it to max and selecting additive alloying (see search strategy below).
* **Filter by material (optional)** whitelist a specific set of materials to only combine those, or blacklist ones you want excluded (e.g. materials you don't have access to).
* **Filter by trait (optional)** only show alloys that end up with specific traits, optionally requiring a minimum trait level.
* **Search strategy** see below. I recommend auto narrow if you are impatient (often doesn't give the best result but is really fast and is usually good enough) or Additive Alloying if you have a couple minutes (In my limited testing it gives the same top results as dynamic filtering and exhaustive searching while being the fastest of the 3).

Click **Apply filters & compute rankings** to run the search. A progress bar shows how it's going larger searches can take a while, especially with big alloy sizes. You can use the estimate button before this to get an estimate of how long the search will take.

### 4. Read the results

Each result card shows:

* The material(s) involved, with the primary material called out.
* A **synergy** tag a multiplier applied to the alloy's stats based on how well the materials mesh together.
* Which machine(s) can actually produce this alloy.
* The computed stat values for your selected role, plus the alloy's resulting traits.
* Other roles the same combination is eligible for, shown as small buttons you can hover over with your mouse to see their stats as well.
* **Average rank** lower is better; this is what results are sorted by.

Use the page selector at the bottom to search through pages of results or jump straight to a specific page number. (Not sure why you wouldn't stay on the first page though)

## Search strategies explained

Checking every possible combination of every material gets combinatorially enormous very fast, so there are a few strategies to choose from depending on how thorough vs. fast you want to be:

* **Auto narrow (recommended)** scores every material by how relevant it is to your selected stats, then keeps adding the best ones to the search pool until it hits a safe computation budget. Fast, and usually finds good (often not perfect) results.
* **Dynamic filter** searches in rounds, growing the alloy size by one each round and trimming out underperforming materials between rounds based on how they did in combination. More thorough than auto narrow but slower.
* **Additive alloying (recommended)** starts with a small brute force search (either size 3 or 4), keeps the best alloys found, then repeatedly tries adding one more material to each survivor and keeps the best results again. (Similar to a beam search)
* **No narrowing (exhaustive)** checks every single valid combination with nothing skipped. Guaranteed to find the true best alloy, but can be extremely slow or even impractical for large material pools or alloy sizes. (I rarely tested this for alloys over size 4 because the computation time goes too large to be practical in any way)

If you're not sure, start with Auto Narrow or Additive Alloying. Use the Estimate time button before committing to a longer search it samples a chunk of the search space and estimates how long the full run will take.





If anything on the website doesn't work correctly, needs better explanation/organization of ui/ux, or have any features to suggest, reach out to me on GitHub and I'll get back to you soon.
