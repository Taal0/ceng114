CENG114 - Computer Programming II Lab Guide 6
Ankara Yıldırım Beyazıt University
Department of Computer Engineering
CENG114 – Computer Programming II
Lab Guide #6: Abstract Classes and Interfaces
Instructor: Yusuf Evren AYKAÇ Week: 9 (Spring 2025–2026)
Assistants: Çağın ÖZKAYA, Hatice UYSAL, Yusuf Ekrem KEÇİLİOĞLU Lab: 6
ℹ Learning Objectives
After completing this lab, you will be able to:
• Define abstract classes with abstract and concrete methods
• Design and implement multiple interfaces
• Apply interface-based polymorphism to decouple behavior from class hierarchy
• Compose objects using HAS-A relationships with interfaces
• Use runtime behavior swapping through setter methods
• Read UML class diagrams and translate them into Java code
Page 1

CENG114 - Computer Programming II Lab Guide 6
Question 1:
DriveEasy is a vehicle rental company operating across Turkey. They need a software system
to manage their diverse fleet of vehicles. Each vehicle type has different rental pricing
strategies, and not all vehicles support the same features (insurance, GPS tracking, etc.).
Your task is to implement the class hierarchy shown in the UML diagram below. Pay close
attention to which classes are abstract, which are interfaces, and which concrete classes
implement which interfaces.
1.1 UML Class Diagram
Figure 1: DriveEasy Araç Kiralama - UML Sınıf Diyagramı
1.2 Interface and Class Specifications
Interface: Rentable
This interface defines the core rental operations that every rentable vehicle must support.
Method Signature Description
rent(String customerName, int days): void Marks vehicle as rented. Print: "[plate] rented to [name]
for [days] days."
returnVehicle(): void Marks vehicle as available. Print: "[plate] has been
returned."
isAvailable(): boolean Returns true if the vehicle is not currently rented.
Page 2

CENG114 - Computer Programming II Lab Guide 6
Interface: Insurable
Vehicles that can be insured implement this interface.
Method Signature Description
calculateInsurance(int days): double Returns the total insurance cost for the given rental
period.
getInsuranceType(): String Returns the insurance category (e.g., "Basic",
"Premium", "Commercial").
Interface: GPSEnabled
Vehicles equipped with GPS navigation implement this interface.
Method Signature Description
activateGPS(): void Activates GPS. Print: "GPS activated for [plate]."
getGPSCost(): double Returns the daily GPS add-on cost.
Abstract Class: Vehicle
The base class for all vehicles in the fleet.
Field / Method Details
- plateNumber: String License plate (set via constructor)
- brand: String Vehicle brand (set via constructor)
- model: String Vehicle model (set via constructor)
- dailyRate: double Base daily rental rate in TL
- year: int Manufacturing year
# rented: boolean Rental status flag (initially false)
abstract calculateRentalCost(int days) Each subclass calculates cost differently
abstract getVehicleCategory() Returns category name as String
getInfo(): String Returns: "[year] [brand] [model] ([plate])"
getDailyRate(): double Returns the base daily rate
toString(): String Returns: "[category]: [info] - [dailyRate] TL/day"
Page 3

CENG114 - Computer Programming II Lab Guide 6
1.3 Concrete Class Details
EconomyCar
Extends Vehicle, implements Rentable and Insurable.
Feature Implementation
Extra field fuelEfficiency: double (km/L)
calculateRentalCost(days) If days >= 7: dailyRate * days * 0.90 (10% discount)Otherwise:
dailyRate * days
getVehicleCategory() Returns "Economy"
calculateInsurance(days) Returns days * 15.0
getInsuranceType() Returns "Basic"
SUV
Extends Vehicle, implements Rentable, Insurable, and GPSEnabled.
Feature Implementation
Extra fields offRoadCapable: boolean, seatingCapacity: int
calculateRentalCost(days) If offRoadCapable: dailyRate * days * 1.15 (15%
surcharge)Otherwise: dailyRate * days
getVehicleCategory() Returns "SUV"
calculateInsurance(days) Returns days * 35.0
getInsuranceType() Returns "Premium"
getGPSCost() Returns 10.0 (per day)
Motorcycle
Extends Vehicle, implements only Rentable. No insurance or GPS support.
Feature Implementation
Extra field engineCC: int (engine displacement)
calculateRentalCost(days) dailyRate * days (flat rate, no discounts)
getVehicleCategory() Returns "Motorcycle"
Van
Extends Vehicle, implements Rentable, Insurable, and GPSEnabled.
Feature Implementation
Extra field cargoCapacityTons: double
calculateRentalCost(days) dailyRate * days + (cargoCapacityTons * 50 * days)
Page 4

CENG114 - Computer Programming II Lab Guide 6
getVehicleCategory() Returns "Commercial Van"
calculateInsurance(days) Returns days * 50.0
getInsuranceType() Returns "Commercial"
getGPSCost() Returns 15.0 (per day)
1.4 Main Class: RentalDemo
In your main method, demonstrate the system as follows:
1. Create one instance of each concrete vehicle type with realistic data.
2. Store all vehicles in a Vehicle[] array. Iterate and print each vehicle using toString().
3. Create a Rentable[] array. Iterate, rent each vehicle to a customer, check availability,
then return.
4. Create an Insurable[] array with only the insurable vehicles. Print insurance details for a
5-day rental.
5. Create a GPSEnabled[] array with GPS-equipped vehicles. Activate GPS and print daily
costs.
6. Calculate and print the total rental cost (rental + insurance + GPS) for each vehicle for a
10-day rental period. Use instanceof checks to determine available features.
⚠ Important Notes for Question 1
• The Motorcycle class does NOT implement Insurable or GPSEnabled. Trying to cast it to these
types should be handled with instanceof.
• The # symbol before rented means protected access in UML.
• Each print message should include the vehicle’s plate number for identification.
• All monetary values should be formatted to 2 decimal places.
Example Run:
========== ALL VEHICLES ==========
Economy: 2023 Toyota Corolla (06 ABC 123) - 450.00 TL/day
SUV: 2024 Jeep Wrangler (34 XY 789) - 850.00 TL/day
Motorcycle: 2022 Honda CBR600 (35 MT 456) - 300.00 TL/day
Commercial Van: 2023 Ford Transit (06 VN 321) - 600.00 TL/day
========== RENTAL OPERATIONS ==========
06 ABC 123 rented to Ahmet Yilmaz for 5 days.
Is 06 ABC 123 available? false
34 XY 789 rented to Elif Demir for 3 days.
Is 34 XY 789 available? false
35 MT 456 rented to Can Ozturk for 7 days.
Is 35 MT 456 available? false
06 VN 321 rented to Zeynep Kaya for 4 days.
Is 06 VN 321 available? false
06 ABC 123 has been returned.
Is 06 ABC 123 available? true
34 XY 789 has been returned.
Is 34 XY 789 available? true
Page 5

CENG114 - Computer Programming II Lab Guide 6
35 MT 456 has been returned.
Is 35 MT 456 available? true
06 VN 321 has been returned.
Is 06 VN 321 available? true
========== INSURANCE DETAILS (5-day rental) ==========
06 ABC 123 - Insurance Type: Basic - Cost: 75.00 TL
34 XY 789 - Insurance Type: Premium - Cost: 175.00 TL
06 VN 321 - Insurance Type: Commercial - Cost: 250.00 TL
========== GPS ACTIVATION ==========
GPS activated for 34 XY 789.
34 XY 789 - Daily GPS Cost: 10.00 TL
GPS activated for 06 VN 321.
06 VN 321 - Daily GPS Cost: 15.00 TL
========== TOTAL COST CALCULATION (10-day rental) ==========
--- 2023 Toyota Corolla (06 ABC 123) ---
Rental Cost : 4050.00 TL (10% discount for 7+ days)
Insurance : 150.00 TL
GPS : N/A
TOTAL : 4200.00 TL
--- 2024 Jeep Wrangler (34 XY 789) ---
Rental Cost : 9775.00 TL (15% off-road surcharge)
Insurance : 350.00 TL
GPS : 100.00 TL
TOTAL : 10225.00 TL
--- 2022 Honda CBR600 (35 MT 456) ---
Rental Cost : 3000.00 TL (flat rate)
Insurance : N/A
GPS : N/A
TOTAL : 3000.00 TL
--- 2023 Ford Transit (06 VN 321) ---
Rental Cost : 8500.00 TL (includes 2500.00 TL cargo surcharge for
2.5 tons)
Insurance : 500.00 TL
GPS : 150.00 TL
TOTAL : 9150.00 TL
Page 6

CENG114 - Computer Programming II Lab Guide 6
Question 2:
You are developing the character system for a role-playing game called DungeonQuest. Each
character has three interchangeable behaviors: how they attack, how they defend, and what
special ability they use.
The key design insight is: behaviors are defined as interfaces and injected into characters
via composition (HAS-A relationship), not inheritance (IS-A). This allows any character to
change their behavior at runtime — a Warrior can learn magic, a Mage can pick up a sword.
🎯 Design Principle
"Program to an interface, not an implementation." Instead of hardcoding behaviors inside each
character class, we define behavior contracts (interfaces) and let concrete strategy classes
implement them. Characters hold references to these interfaces and delegate behavior to them. This
makes the system open for extension but closed for modification.
2.1 UML Class Diagram
Figure 2: DungeonQuest Oyunu - UML Sınıf Diyagramı
2.2 Interface Specifications
Interface: AttackBehavior
Method Description
performAttack(): String Returns a description of the attack (e.g., "Swings a mighty
sword!")
getDamage(): int Returns the base damage value of this attack
Interface: DefenseBehavior
Method Description
performDefense(): String Returns a description of the defense action
Page 7

CENG114 - Computer Programming II Lab Guide 6
| getDefensePoint(): int  |     | Returns the defense point value  |     |     |
| ----------------------- | --- | -------------------------------- | --- | --- |

Interface: SpecialAbility
| Method  |     | Description  |     |     |
| ------- | --- | ------------ | --- | --- |
useAbility(): String  Returns a description of the special ability activation
| getCooldown(): int  |     | Returns number of turns before reuse  |     |     |
| ------------------- | --- | ------------------------------------- | --- | --- |
| getManaCost(): int  |     | Returns mana cost to activate         |     |     |
2.3  Concrete Behavior Classes
Attack Behaviors
| Class       | implements      | performAttack() returns   |     | getDamage()  |
| ----------- | --------------- | ------------------------- | --- | ------------ |
| SwordSlash  | AttackBehavior  | "Swings a mighty sword!"  |     | 25           |
MagicMissile  AttackBehavior  "Launches a glowing magic missile!"  30
| ArrowShot  | AttackBehavior  | "Fires a precise arrow!"  |     | 20  |
| ---------- | --------------- | ------------------------- | --- | --- |

Defense Behaviors
Class  implements  performDefense() returns  getDefensePoint()
ShieldBlock  DefenseBehavior  "Raises shield to block the attack!"  20
| DodgeRoll  | DefenseBehavior  | "Rolls sideways to dodge!"  |     | 15  |
| ---------- | ---------------- | --------------------------- | --- | --- |
MagicBarrier  DefenseBehavior  "Conjures a shimmering magic  25
barrier!"

Special Abilities
| Class  | implements  | useAbility() returns  | Mana  | Cooldown  |
| ------ | ----------- | --------------------- | ----- | --------- |
Fireball  SpecialAbility  "Hurls a massive fireball!"  30  3 turns
HealingLight  SpecialAbility  "A warm light heals all wounds!"  25  2 turns
StealthMode  SpecialAbility  "Vanishes into the shadows!"  20  4 turns

2.4  Abstract Class: GameCharacter
The central abstract class that holds behavior references. Note the composition relationship:
GameCharacter does not extend the behavior interfaces; it contains them as instance
variables.
Page 8

CENG114 - Computer Programming II Lab Guide 6
Member  Details
| - name: String                    |     | Character name                            |     |     |
| --------------------------------- | --- | ----------------------------------------- | --- | --- |
| - health: int                     |     | Current health points                     |     |     |
| - level: int                      |     | Character level (starts at 1)             |     |     |
| - attackBehavior: AttackBehavior  |     | Current attack strategy (interface type)  |     |     |
- defenseBehavior:  Current defense strategy (interface type)
DefenseBehavior
| - specialAbility: SpecialAbility  |     | Current special ability (interface type)  |     |     |
| --------------------------------- | --- | ----------------------------------------- | --- | --- |
Constructor  GameCharacter(name, health, level, attackBehavior,
defenseBehavior, specialAbility)
attack(): String  Delegates to attackBehavior.performAttack() — returns: name + "
attacks! " + result
defend(): String  Delegates to defenseBehavior.performDefense() — returns: name
+ " defends! " + result
useSpecial(): String  Delegates to specialAbility.useAbility() — returns: name + " uses
special! " + result
| setAttackBehavior(a)          |     | Changes attack behavior at runtime    |     |     |
| ----------------------------- | --- | ------------------------------------- | --- | --- |
| setDefenseBehavior(d)         |     | Changes defense behavior at runtime   |     |     |
| setSpecialAbility(s)          |     | Changes special ability at runtime    |     |     |
| abstract levelUp()            |     | Each subclass implements differently  |     |     |
| abstract getCharacterClass()  |     | Returns class name as String          |     |     |
getStatus(): String  Returns: "[class] [name] - HP: [health], Level: [level]"
2.5  Concrete Character Classes
| Class  | Extra Field  |     | levelUp() Effect  | getCharacterClass()  |
| ------ | ------------ | --- | ----------------- | -------------------- |
Warrior  rage: int  health += 20, rage += 10, level++  "Warrior"
| Mage  | mana: int  | health += 10, mana += 20, level++  |     | "Mage"  |
| ----- | ---------- | ---------------------------------- | --- | ------- |
Archer  agility: int  health += 15, agility += 15, level++  "Archer"
Each character class has a constructor that calls super(...) and also initializes its unique field.
For example, Warrior's constructor:
public Warrior(String name, int health, int level,
               AttackBehavior atk, DefenseBehavior def,
               SpecialAbility spc, int rage) {
    super(name, health, level, atk, def, spc);
    this.rage = rage;
}

Page 9

CENG114 - Computer Programming II Lab Guide 6
2.6 Main Class: GameDemo
In your main method:
1. Create behavior objects: at least one of each concrete attack, defense, and special
ability.
2. Create three characters — a Warrior (SwordSlash + ShieldBlock + Fireball), a Mage
(MagicMissile + MagicBarrier + HealingLight), and an Archer (ArrowShot + DodgeRoll +
StealthMode).
3. Store characters in a GameCharacter[] array. Iterate and print each character’s status
using getStatus().
4. For each character, call attack(), defend(), and useSpecial(). Print all results.
5. RUNTIME BEHAVIOR SWAP: The Warrior learns magic! Use setAttackBehavior() to
change the Warrior’s attack to MagicMissile. Then call attack() again and print the result
to show the behavior has changed.
6. Level up each character twice. Print their updated status after each levelUp() call.
7. Create a new Warrior with ArrowShot + MagicBarrier + HealingLight (a hybrid build!).
Demonstrate that any character can use any combination of behaviors.
⚠ Important Notes for Question 2
• The ◆ (filled diamond) in the UML diagram represents composition — GameCharacter OWNS the
behavior objects.
• Notice that behaviors are stored as INTERFACE types, not concrete types. This is what enables
runtime swapping.
• The setter methods (setAttackBehavior, etc.) are the key to runtime flexibility. Without them,
behaviors would be fixed at construction time.
• Think about WHY this design is better than putting all attack/defense logic directly in each
character subclass. What if you needed to add a new weapon type?
Example Run:
========== CHARACTER STATUS ==========
Warrior Thorin - HP: 120, Level: 1
Mage Elara - HP: 80, Level: 1
Archer Legolas - HP: 100, Level: 1
========== BATTLE ACTIONS ==========
--- Thorin's Turn ---
Thorin attacks! Swings a mighty sword! (Damage: 25)
Thorin defends! Raises shield to block the attack! (Defense: 20)
Thorin uses special! Hurls a massive fireball! (Mana: 30, Cooldown: 3
turns)
--- Elara's Turn ---
Elara attacks! Launches a glowing magic missile! (Damage: 30)
Elara defends! Conjures a shimmering magic barrier! (Defense: 25)
Elara uses special! A warm light heals all wounds! (Mana: 25,
Cooldown: 2 turns)
--- Legolas's Turn ---
Legolas attacks! Fires a precise arrow! (Damage: 20)
Page 10

CENG114 - Computer Programming II Lab Guide 6
Legolas defends! Rolls sideways to dodge! (Defense: 15)
Legolas uses special! Vanishes into the shadows! (Mana: 20, Cooldown:
4 turns)
========== RUNTIME BEHAVIOR SWAP ==========
Thorin the Warrior learns magic!
[Before] Thorin attacks! Swings a mighty sword! (Damage: 25)
* Changing attack behavior to MagicMissile... *
[After] Thorin attacks! Launches a glowing magic missile! (Damage:
30)
========== LEVEL UP ==========
--- Round 1 ---
Thorin leveled up! HP: 140, Rage: 60, Level: 2
Elara leveled up! HP: 90, Mana: 120, Level: 2
Legolas leveled up! HP: 115, Agility: 65, Level: 2
--- Round 2 ---
Thorin leveled up! HP: 160, Rage: 70, Level: 3
Elara leveled up! HP: 100, Mana: 140, Level: 3
Legolas leveled up! HP: 130, Agility: 80, Level: 3
========== UPDATED STATUS ==========
Warrior Thorin - HP: 160, Level: 3
Mage Elara - HP: 100, Level: 3
Archer Legolas - HP: 130, Level: 3
========== HYBRID CHARACTER ==========
Creating hybrid Warrior: Kael (ArrowShot + MagicBarrier +
HealingLight)
Warrior Kael - HP: 110, Level: 1
Kael attacks! Fires a precise arrow! (Damage: 20)
Kael defends! Conjures a shimmering magic barrier! (Defense: 25)
Kael uses special! A warm light heals all wounds! (Mana: 25,
Cooldown: 2 turns)
* Any character can use ANY combination of behaviors! *
Page 11