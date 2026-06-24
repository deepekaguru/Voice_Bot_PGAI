# Bug Report: Pretty Good AI - Voice Agent

**Test Number:** +18054398008  
**Calling Number:** +19714369290  
**Testing Period:** June 20-23, 2026  
**Total Calls Made:** 15 +

---

## Bug 1: Escalation Reset Loop

**Frequency:** Reproduced 6+ times across multiple scenarios  
**Scenarios affected:** scheduling, reschedule, refill, inquiry,
edge_change_mind, edge_vague  

**Description:**  
Every time the agent attempts to transfer the caller to a
representative or clinic support team, the call does not actually
transfer. Instead, the call resets entirely to the opening greeting:
"Hello, you've reached the Pretty Good AI test line. Goodbye." The
caller is disconnected without ever reaching a
representative.

**Steps to reproduce:**  
1. Call the test line and engage in any scenario
2. Trigger any situation where the agent offers to connect to a
   representative or clinic support team
3. Agent says "Connecting you to a representative. Please wait."
4. Call resets to opening IVR greeting instead of transferring


**Call references:**  
transcripts/refill_01.txt

---

## Bug 2: Agent Contradicts Itself on Appointment Record Access

**Frequency:** Observed in scheduling scenario  
**Scenarios affected:** scheduling  

**Description:**  
Within the same call, the agent first confirmed it could see
an existing appointment ("It looks like you already have a
routine checkup appointment booked") but then immediately
said "I don't have access to your current appointment details"
when the patient asked for the date and time. The agent
acknowledged the appointment existed but then denied being
able to access its details.

**Steps to reproduce:**  
1. Call and request to book a routine appointment
2. Agent confirms an existing appointment is on file
3. Ask the agent for the date and time of that appointment
4. Agent says it cannot access the appointment details

**Call references:**  
 transcripts/scheduling_20260623_0104521.txt
 
---

## Bug 3: Agent Sends Confirmation Text Without Patient Consent

**Frequency:** Observed in reschedule scenario  
**Scenarios affected:** reschedule  

**Description:**  
When asked if they wanted appointment details sent via text,
the patient declined. The agent sent the text message anyway
without acknowledging the patient's response.

**Steps to reproduce:**  
1. Call and successfully reschedule an appointment
2. When agent asks "Would you like me to send you a text
   message with these details?" respond with no
3. Agent sends the text anyway

**Call references:** 

transcripts/scheduling_20260623_0104521.txt

---
## Bug 4: Agent Offers Live Support Then Ignores Patient Choice

**Frequency:** Multiple occurrences  
**Scenarios affected:** scheduling, refill, billing_dispute  

**Description:**  
The agent says "I can connect you to our clinic support team,
however I'm a Pretty Good AI and can do many of the things an
operator can. Do you want to give me a try?" followed immediately
by "Connecting you to a representative. Please wait." without
waiting for the caller's response.

**Steps to reproduce:**  
1. Call and reach a point where the agent offers to connect
   to support
2. Agent offers itself as an alternative to a human operator
3. Without waiting for response, agent connects to
   representative anyway


---

## Bug 5: Appointment Confirmation Delivered After Call Disconnects

**Frequency:** Observed in reschedule and scheduling scenarios  
**Scenarios affected:** reschedule, scheduling  

**Description:**  
The agent's final appointment confirmation message is delivered after
the Twilio stream has already closed and the call has ended. The
caller hangs up without ever hearing the confirmation of their
appointment change or booking.

**Steps to reproduce:**  
1. Call and successfully reschedule or book an appointment
2. Agent says "You're all set" and call ends
3. Final confirmation message arrives after the stream has
   already closed 

**Call references:**  
This message does not appear in the saved transcript since it
arrives after the stream closes. It is audible in the recording at
the end of the call after the bot's closing line.

---

## Bug 6: Agent Loses Internal State Mid-Call

**Frequency:** Observed in refill scenario
**Scenarios affected:** refill

**Description:**
During the refill scenario, the agent appeared to lose track
of what it was doing mid-conversation. It said "I'm connecting
you to our clinic support team now," then immediately said
"Let me process that now" as if starting a new action, then
said "I'm connecting you to our clinic support team now" again.
The agent cycled through the same statements repeatedly without
making progress, suggesting an internal state management issue.

**Steps to reproduce:**
1. Call and request a medication refill
2. Provide medication name, days remaining, callback number,
   and pharmacy details
3. Agent attempts to process the request and gets stuck
   in a loop


**Call references:**  
transcripts/refill_02.txt

---

## Bug 7: DOB Validation Failing

**Frequency:** Every single call  
**Scenarios affected:** All  

**Description:**  
The agent's date of birth validation behaves inconsistently
across calls. In most calls, regardless of the date of birth
provided, the agent responds with "The birthday doesn't match
our records, but for demo purposes, I'll accept it,". However in some
calls the DOB was accepted without this message, suggesting
the validation logic is unreliable and non-deterministic.

**Steps to reproduce:**  
1. Call the test line
2. Provide any date of birth when asked
3. Agent responds with the demo purposes message regardless


**Call references:**  
All transcripts

---





