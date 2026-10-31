# Ex.No-09-Building-a-Simple-AI-Agent-AI-Tourist-Guiide-for-India
## Aim : To design, implement and test a simple goal-based AI agent in Python that plans a personalised India trip itinerary for a tourist, based on the tourist's interest, trip duration and daily budget.
Introduction
An AI agent is anything that can perceive its environment through sensors and act upon that environment through actuators in order to achieve a specific goal. A useful way to describe an agent is the PEAS framework — Performance measure, Environment, Actuators and Sensors. Agents are commonly classified as simple reflex agents (react only to the current input), goal-based agents (choose actions that achieve a defined goal) and utility-based agents (choose the action that maximises a measure of “goodness”). In this experiment, an AI Tourist Agent for India is built as a goal-based agent: given a tourist's goal (an enjoyable trip within their interest and budget), the agent perceives the tourist's preferences, reasons over a knowledge base of Indian destinations, plans a day-wise itinerary, and acts by presenting the recommended plan.
Procedure
### Step 1: Import Required Libraries
●	textwrap – used only to neatly wrap long destination descriptions to a fixed line width while printing the itinerary.
<img width="584" height="34" alt="image" src="https://github.com/user-attachments/assets/d7bd1246-346f-41f8-aec1-fddca7a8e120" />
### Step 2: Define the Agent's Knowledge Base (Destination Database)
●	The agent's environment knowledge is stored as a list of dictionaries, one per Indian destination.
●	Each destination has a category (heritage, spiritual, beach, hill_station, adventure, nature, wildlife), an estimated cost per day, the best travel season and a short description.
●	This knowledge base plays the same role for the agent that the environment model plays for any goal-based agent — it is what the agent reasons over to choose its actions.
<img width="619" height="236" alt="image" src="https://github.com/user-attachments/assets/4182dfe7-9114-4557-a80f-a8756a48b2f5" />
<img width="615" height="347" alt="image" src="https://github.com/user-attachments/assets/320ffc13-4890-4761-9499-34426c329b35" />
Destination Knowledge Base Summary
The table below summarises the destinations available to the agent:
<img width="673" height="213" alt="image" src="https://github.com/user-attachments/assets/c5fea0c9-95f2-482f-a1fb-d9cfafdcf2ea" />
### Step 3: Perceive — Read the Tourist's Goal / Preferences
●	The perceive() function represents the agent's sensors: it reads the tourist's profile (interest category, number of days, daily budget) and displays it back to confirm what was understood.
<img width="585" height="109" alt="image" src="https://github.com/user-attachments/assets/81c0cc39-ba1f-4736-8a51-1eb1ba2ff3f8" />
### Step 4: Reason — Filter and Rank Matching Destinations
●	The agent compares every destination in its knowledge base against the tourist's goal: same category and cost per day within the daily budget.
●	If nothing fits the exact budget, it falls back to all destinations of the requested category, sorted from cheapest to costliest, so the agent always tries to return a useful plan.
<img width="615" height="148" alt="image" src="https://github.com/user-attachments/assets/ba53e68e-5fb0-4673-83cf-5b705751df15" />
### Step 5: Plan — Build a Day-wise Itinerary
●	The agent allocates up to 3 days per destination and moves to the next matching destination once those days are used, cycling back to the first if needed for longer trips.
●	Consecutive days at the same destination are merged into a single itinerary block, and the total estimated cost is calculated as it plans.
<img width="612" height="290" alt="image" src="https://github.com/user-attachments/assets/f793c485-908e-4e98-84ca-119671909652" />
### Step 6: Act — Present the Recommendation to the Tourist
●	The act() function represents the agent's actuator: it prints a readable, day-wise itinerary with the destination, description, best season and cost for each leg of the trip, followed by the total estimated cost.
<img width="576" height="177" alt="image" src="https://github.com/user-attachments/assets/55712bdf-2b82-42b0-83f7-5b3eb7d94263" />
<img width="632" height="89" alt="image" src="https://github.com/user-attachments/assets/9a254225-96b6-4729-be35-29cac298c28d" />
### Step 7: The Agent Loop — Perceive → Reason → Plan → Act
●	run_agent() ties the four stages together into a single agent cycle, exactly as a real autonomous agent continuously perceives, reasons and acts within its environment.
<img width="432" height="89" alt="image" src="https://github.com/user-attachments/assets/0a46568e-9781-457b-8a10-e2a97a95c0a2" />
### Step 8: Test the Agent with Sample Tourist Profiles
●	Three realistic tourist profiles are used to test the agent across different interests and budgets: a budget heritage traveller, a mid-budget adventure seeker, and a family looking for a beach holiday.
<img width="616" height="241" alt="image" src="https://github.com/user-attachments/assets/17613086-5b61-44f1-8d85-cbbae85cfc4b" />

**PROGRAM**
```
import textwrap

destinations = [
    {
        "name": "Taj Mahal, Agra",
        "state": "Uttar Pradesh",
        "category": "heritage",
        "cost_per_day": 2500,
        "season": "Oct-Mar",
        "desc": "Iconic Mughal-era marble mausoleum, a UNESCO World Heritage site."
    },
    {
        "name": "Jaipur City",
        "state": "Rajasthan",
        "category": "heritage",
        "cost_per_day": 2800,
        "season": "Oct-Mar",
        "desc": "The Pink City with Amber Fort, Hawa Mahal and royal palaces."
    },
    {
        "name": "Goa Beaches",
        "state": "Goa",
        "category": "beach",
        "cost_per_day": 3500,
        "season": "Nov-Feb",
        "desc": "Golden beaches, nightlife and water sports."
    },
    {
        "name": "Andaman Islands",
        "state": "Andaman & Nicobar",
        "category": "beach",
        "cost_per_day": 5500,
        "season": "Nov-Apr",
        "desc": "Pristine beaches, coral reefs and scuba diving."
    },
    {
        "name": "Manali",
        "state": "Himachal Pradesh",
        "category": "hill_station",
        "cost_per_day": 3000,
        "season": "Mar-Jun, Dec-Jan",
        "desc": "Himalayan hill town popular for scenic views and adventure sports."
    },
    {
        "name": "Ladakh",
        "state": "Ladakh",
        "category": "adventure",
        "cost_per_day": 4500,
        "season": "May-Sep",
        "desc": "High-altitude desert with monasteries, mountains and Pangong Lake."
    },
    {
        "name": "Spiti Valley",
        "state": "Himachal Pradesh",
        "category": "adventure",
        "cost_per_day": 4000,
        "season": "May-Oct",
        "desc": "Cold-desert valley popular for trekking and mountain biking."
    },
    {
        "name": "Coorg",
        "state": "Karnataka",
        "category": "adventure",
        "cost_per_day": 3200,
        "season": "Oct-Mar",
        "desc": "Coffee-plantation hills, trekking and waterfalls."
    },
    {
        "name": "Kerala Backwaters",
        "state": "Kerala",
        "category": "nature",
        "cost_per_day": 4000,
        "season": "Sep-Mar",
        "desc": "Houseboat cruises through peaceful backwaters and lagoons."
    },
    {
        "name": "Ranthambore National Park",
        "state": "Rajasthan",
        "category": "wildlife",
        "cost_per_day": 4800,
        "season": "Oct-Jun",
        "desc": "One of India's best parks for spotting wild tigers."
    },
    {
        "name": "Varanasi",
        "state": "Uttar Pradesh",
        "category": "spiritual",
        "cost_per_day": 2000,
        "season": "Oct-Mar",
        "desc": "Ancient spiritual city famous for the Ganges ghats and evening Ganga Aarti."
    },
    {
        "name": "Rishikesh",
        "state": "Uttarakhand",
        "category": "spiritual",
        "cost_per_day": 2200,
        "season": "Sep-Mar",
        "desc": "Spiritual destination known for yoga, temples and river-side activities."
    }
]


def perceive(profile):
    print("\n" + "=" * 65)
    print("TOURIST PROFILE")
    print("=" * 65)
    print(f"Tourist Name   : {profile['name']}")
    print(f"Interest       : {profile['category']}")
    print(f"Trip Duration  : {profile['days']} days")
    print(f"Daily Budget   : Rs. {profile['budget_per_day']}")
    return profile


def reason(profile):
    matches = [
        destination
        for destination in destinations
        if destination["category"] == profile["category"]
        and destination["cost_per_day"] <= profile["budget_per_day"]
    ]

    if not matches:
        matches = [
            destination
            for destination in destinations
            if destination["category"] == profile["category"]
        ]

    matches.sort(
        key=lambda destination: destination["cost_per_day"]
    )

    return matches


def plan(profile, matches):
    if not matches:
        return [], 0

    days_left = profile["days"]
    itinerary = []
    index = 0

    while days_left > 0 and matches:
        destination = matches[index % len(matches)]
        days_here = min(3, days_left)

        if (
            itinerary
            and itinerary[-1]["destination"]["name"]
            == destination["name"]
        ):
            itinerary[-1]["days"] += days_here
        else:
            itinerary.append({
                "destination": destination,
                "days": days_here
            })

        days_left -= days_here
        index += 1

    total_cost = sum(
        item["days"] * item["destination"]["cost_per_day"]
        for item in itinerary
    )

    return itinerary, total_cost


def act(itinerary, total_cost, profile):
    if not itinerary:
        print("\nSorry, no destinations match this profile.")
        return

    print("\n" + "=" * 65)
    print("RECOMMENDED ITINERARY")
    print("=" * 65)

    day_counter = 1

    for item in itinerary:
        destination = item["destination"]

        start_day = day_counter
        end_day = day_counter + item["days"] - 1

        if start_day == end_day:
            day_text = f"Day {start_day}"
        else:
            day_text = f"Days {start_day}-{end_day}"

        print(f"\n{day_text}: {destination['name']}")
        print(f"State       : {destination['state']}")
        print(f"Category    : {destination['category']}")
        print(f"Cost/Day    : Rs. {destination['cost_per_day']}")
        print(f"Best Season : {destination['season']}")

        print(
            "Description :",
            textwrap.fill(
                destination["desc"],
                width=60
            )
        )

        print(
            f"Estimated Cost for this stay: "
            f"Rs. {item['days'] * destination['cost_per_day']}"
        )

        day_counter += item["days"]

    print("\n" + "-" * 65)
    print(f"Total Estimated Trip Cost: Rs. {total_cost}")
    print(f"Trip Duration: {profile['days']} days")
    print("=" * 65)


def run_agent(profile):
    profile = perceive(profile)

    matches = reason(profile)

    print("\nMatching Destinations:")

    if matches:
        for destination in matches:
            print(
                f"- {destination['name']} "
                f"(Rs. {destination['cost_per_day']}/day)"
            )
    else:
        print("No matching destinations found.")

    itinerary, total_cost = plan(
        profile,
        matches
    )

    act(
        itinerary,
        total_cost,
        profile
    )


sample_profiles = [
    {
        "name": "Ananya (Budget Heritage Traveller)",
        "category": "heritage",
        "days": 6,
        "budget_per_day": 3000
    },
    {
        "name": "Rahul (Mid-Budget Adventure Seeker)",
        "category": "adventure",
        "days": 5,
        "budget_per_day": 5000
    },
    {
        "name": "The Fernandes Family (Beach Holiday)",
        "category": "beach",
        "days": 7,
        "budget_per_day": 6000
    }
]


print("\n")
print("=" * 65)
print("              AI TOURIST AGENT FOR INDIA")
print("=" * 65)
print("\nGoal-Based Agent: Perceive -> Reason -> Plan -> Act")

for index, profile in enumerate(
    sample_profiles,
    start=1
):
    print("\n")
    print("#" * 65)
    print(f"SESSION {index}")
    print("#" * 65)

    run_agent(profile)

    print("\n" + "-" * 65)

print("\n")
print("=" * 65)
print("              ALL SESSIONS COMPLETED")
print("=" * 65)
```

### Output

**Agent Output – Session 1**

The agent processes the heritage traveller's profile and generates a suitable itinerary based on the specified duration and daily budget.

<img width="1002" height="994" alt="image" src="https://github.com/user-attachments/assets/36a3f7c4-756a-457f-a3f2-30aca3c21c96" />

**Agent Output – Session 2**

The agent processes the adventure traveller's profile and generates a suitable itinerary using matching adventure destinations.

<img width="1014" height="934" alt="image" src="https://github.com/user-attachments/assets/6babd7e3-a055-47b1-8bfe-1d4b1f36bb6f" />

**Agent Output – Session 3**

The agent processes the beach holiday profile and generates a suitable itinerary while calculating the estimated total trip cost.

<img width="987" height="1016" alt="image" src="https://github.com/user-attachments/assets/07519436-00ce-42bb-a038-3af5fb49dbcc" />

**OBSERVATION**

The AI Tourist Agent successfully processes different tourist profiles and generates travel recommendations based on their interest, trip duration, and daily budget.

The agent uses a predefined destination knowledge base and goal-based reasoning to identify suitable destinations.

The estimated cost is automatically calculated using the number of days spent at each destination and its estimated daily cost.

**RESULT**

The simple goal-based AI Tourist Guide for India was successfully designed, implemented, and tested using Python.

The agent successfully generated personalized travel itineraries for different tourist profiles based on their interests, trip duration, and daily budget.


## Conclusion
Thus, a simple goal-based AI Tourist Agent for India was successfully designed, implemented and tested using Python. The agent follows the classic Perceive → Reason → Plan → Act cycle: it perceives a tourist's goal (interest, duration and budget), reasons over a knowledge base of Indian destinations to find matching options, plans a day-wise itinerary, and acts by presenting a complete, costed trip recommendation. This experiment demonstrates the core building blocks of autonomous agents — environment knowledge, perception, reasoning/planning and action — on which more advanced AI agents (using machine learning, real-time APIs and large language models) are built.

