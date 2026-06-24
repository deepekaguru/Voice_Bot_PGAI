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

**Call references:**  
transcripts/reschedule_20260623_005751.txt

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

## Bug 6: IVR Greeting Replays Mid-Call

**Frequency:** Observed in refill scenario  
**Scenarios affected:** refill  

**Description:**  
During an active call, the full IVR opening message replayed
in the middle of the conversation, interrupting the agent
mid-sentence. This confirms that the transfer/escalation
mechanism restarts the entire call flow rather than connecting
to a different queue.

**Steps to reproduce:**  
1. Call and engage in refill scenario
2. Reach the point where agent attempts to process the refill
3. IVR greeting replays mid-call


**Call references:**  
transcripts/refill_02.txt


---

## Bug 7: DOB Validation Leaks Internal Demo Logic

**Frequency:** Every single call  
**Scenarios affected:** All  

**Description:**  
After the caller provides their date of birth, the agent responds
with: "The birthday doesn't match our records, but for demo purposes,
I'll accept it." This message exposes internal demo/testing logic
directly to the caller. In a production environment, real patients
would see this message, which is both confusing and unprofessional.
It also reveals that identity verification is not actually being
enforced, which is a significant security and compliance concern for
a HIPAA-adjacent healthcare product.

**Steps to reproduce:**  
1. Call the test line
2. Provide any date of birth when asked
3. Agent responds with the demo purposes message regardless


**Call references:**  
All transcripts

---





