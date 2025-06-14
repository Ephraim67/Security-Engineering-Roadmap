## **What They’re Trying to Achieve**

The client wants to **remotely deploy a digital in-room experience** for hotel guests that:

1. **Welcomes guests by name on TV or smart displays**
2. Shows **hotel services**, promotions, menus, etc.
3. Integrates with the **Property Management System (PMS)** (e.g., Opera, Maestro) to automate guest data and room information.
4. Provides **entertainment options** (IPTV, streaming, etc.)
5. Ensures hotel staff can **support and manage** the system without you after deployment.

Think of it as transforming a standard TV into a smart concierge and entertainment hub for each hotel room.

---

## 🔧 Let's Use **Nonius** as Our Example Tool

**Nonius** is a widely-used hospitality system that offers:

* IPTV
* Digital signage
* Guest messaging
* PMS integration

We’ll walk through **a step-by-step deployment** using Nonius and the full setup checklist.

---

##  **Full Step-by-Step Deployment Plan: Nonius IPTV System**

###  PHASE 1: Preparation

| Step | Task                               | Details                                                   |
| ---- | ---------------------------------- | --------------------------------------------------------- |
| 1    | Contact hotel IT                   | Ask about current TV system, network, and infrastructure  |
| 2    | Gather guest data integration info | What PMS is used? (e.g., Opera PMS)                       |
| 3    | Get remote access                  | VPN, RDP, or Nonius Central portal access                 |
| 4    | Verify hardware                    | TV model or set-top boxes supported by Nonius             |
| 5    | Confirm branding content           | Hotel logo, welcome text, services menu, language options |



###  PHASE 2: Server & System Installation

| Step | Task                                               | Tool / Portal                                                        |
| ---- | -------------------------------------------------- | -------------------------------------------------------------------- |
| 1    | Connect to hotel network via VPN                   | Cisco AnyConnect or OpenVPN                                          |
| 2    | Log in to Nonius Admin Panel (usually via browser) | [https://admin.noniussoftware.com](https://admin.noniussoftware.com) |
| 3    | Configure backend server settings                  | Set server IP, DNS, DHCP (if needed)                                 |
| 4    | Install or update Nonius firmware on guest devices | Push via Admin Panel                                                 |



###  PHASE 3: PMS Integration

| Step | Task                                                | Notes                                                  |
| ---- | --------------------------------------------------- | ------------------------------------------------------ |
| 1    | In Admin Panel, go to **"PMS Integration"** section | Choose system (e.g., Opera)                            |
| 2    | Enter API credentials or connection string          | Provided by hotel IT or PMS vendor                     |
| 3    | Map fields: guest name, check-in date, room number  | This enables personalized TV greetings                 |
| 4    | Test integration                                    | Simulate check-in to verify guest data populates on TV |



###  PHASE 4: Customization & Branding

| Step | Task                               | Details                                      |
| ---- | ---------------------------------- | -------------------------------------------- |
| 1    | Upload hotel logo and color scheme | PNG/JPG                                      |
| 2    | Configure guest home screen layout | Welcome message, room number, check-out date |
| 3    | Add service menus                  | Room service, laundry, spa booking, etc.     |
| 4    | Add language options               | English, French, etc.                        |



###  PHASE 5: Room Device Setup

| Step | Task                                          | Instructions                                 |
| ---- | --------------------------------------------- | -------------------------------------------- |
| 1    | Remotely detect/set up all TVs in guest rooms | Via Admin Panel → Devices                    |
| 2    | Assign each TV a room number and profile      | Based on PMS mapping                         |
| 3    | Run configuration test per room               | TV should show guest info and welcome screen |
| 4    | Schedule content updates                      | Promotions, messages, etc.                   |



###  PHASE 6: Testing

| Step | Task                            | Test Scope                                |
| ---- | ------------------------------- | ----------------------------------------- |
| 1    | Perform a test check-in via PMS | Should update guest screen instantly      |
| 2    | Switch languages                | Check translation is working              |
| 3    | Navigate menus using remote     | Services should load correctly            |
| 4    | Test IPTV streams               | Channels load smoothly, no buffering      |
| 5    | Simulate a check-out            | Screen should reset to idle/default state |


###  PHASE 7: Staff Training & Handoff

| Step | Task                              | Notes                                        |
| ---- | --------------------------------- | -------------------------------------------- |
| 1    | Host a Zoom call with hotel staff | Demo Admin Panel and TV usage                |
| 2    | Share training manual             | PDF or Google Doc                            |
| 3    | Show how to upload/update promos  | e.g., Happy Hour ads or Spa Discounts        |
| 4    | Finalize support contact          | Your email, response SLA, or escalation path |



### PHASE 8: Documentation

Prepare and share:

* A PDF with deployment steps + screenshots
* Support/troubleshooting guide (e.g., how to reboot a TV box remotely)
* Admin access credentials (if allowed)
* System diagram (TVs, server, PMS, access control)



##  Tip for You as the Engineer

* Use **Notion or Google Docs** to manage the project steps and share live updates with the client.
* Create a reusable **deployment checklist** so every hotel you work with can follow the same process with minor edits.
