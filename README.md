# AI-Actor-Headshot-Submission-Agent
Increase actor's casting call backs by picking the right headshot for the right role.

## How to Use

1. Open [n8n](https://n8n.io).
2. In the workflow editor, click the **three-dot menu (⋮)** → **Import from File**.
3. Upload the file: `AI-Archetype-Identifier.json` from this repo.
4. Add your API keys (OpenAI, Google Sheets, etc.) inside n8n’s **Credentials** section.
5. Click **Execute Workflow** to run the agent and see your top 3 headshot matches.

### Setting up Google Sheets for workflow’s Build Catalog node

Required headers (exactly):
Headshot_ID – a unique ID per headshot (e.g., MJ-049)
Archetype – short label (e.g., Velvet Enforcer)
Subtype – short label (e.g., The Iron Sentinel)
ArchetypeSubtypeEssence – text blob of key phrases (comma/semicolon separated)
Best_for_Roles – text blob of role phrases (comma/semicolon separated)
Keywords – text blob of keywords (comma/semicolon separated)

#### How to format the three “array” fields (Essence / Roles / Keywords):

Put short phrases separated by commas or semicolons in a single cell.
(The scoring code treats the whole cell as text and matches your phrases against the parsed JSON arrays.) 
Keep phrases concise (nouns/adjectives/2–3 words). Your parser is already tuned to produce arrays in that style. 
One row = one headshot/look.

Example: 
Headshot_ID: MJ-049
Archetype: Velvet Enforcer
Subtype: The Iron Sentinel
ArchetypeSubtypeEssence: quiet command; stoic restraint; watchful; dignified protector
Best_for_Roles: law enforcement; military strategist; disciplinarian teacher; community protector
Keywords: stoic; watchful; protector; authority; unyielding; boundaries; command; composed

Where these fields are used

The parser creates:
archetype (string), subtype (string), archetype_subtype_essence (array), best_for_roles (array), keywords (array). 

The Top 3 Headshots node scores each sheet row with weights:
essence 0.45, bestFor 0.20, keywords 0.20, archetype 0.10, subtype 0.05.
(It blends exact phrase hits with token overlap and then ranks the top 3.)

##### Practical tips

Make sure the first row is headers and there are no blank rows in the sheet segment the node reads.
Keep Headshot_ID unique; that’s what gets returned in the final ranking. 
If you rename the sheet or move to a new doc, update the Read Archetype Map node (Document ID and Sheet) accordingly. 
You can expand optional columns like Expression, Energy, Presence, or Notes for your own reference—they won’t affect scoring unless you map them into the catalog code. (Those headers are already present in your headshot map.) 
