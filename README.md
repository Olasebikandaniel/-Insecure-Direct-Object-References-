# PortSwigger Web Security Academy: Insecure Direct Object References (IDOR) Lab –solved

## Lab Overview
**Difficulty:** Apprentice  
**Category:** Access Control → Insecure Direct Object References  
**Goal:** Retrieve the password for user `carlos` and log in to his account.

The app stores live chat transcripts as static `.txt` files on the server filesystem, named with incrementing numbers (e.g., `2.txt` for your current session). No authorization check – if you know (or guess) the filename, you read it. Textbook IDOR.

## Exploitation Steps (How I Popped It)
1. Logged in as regular user (wiener/peter or whatever the lab gave).
2. Went to **Live chat** → sent a dummy message → clicked **View transcript**.
3. Captured the download request in Burp → saw `GET /download-transcript/2.txt`.
4. In Repeater, changed `2.txt` → `1.txt` (previous chat log).
5. Response dropped the full transcript → Hal Pline (the bot) asking for password confirmation → user straight-up replies with Carlos' password in clear text.
6. Went back to login page → `carlos` + leaked password → access granted → lab solved.

## Vulnerability Breakdown
- Direct object reference: `/download-transcript/[number].txt`
- No server-side check: "Is this number tied to the current authenticated user?"
- Result: Horizontal privilege escalation – low-priv user reads high-priv (or any other) user's sensitive data.

## Lessons Carved Into My Brain
- Predictable identifiers = attacker candy.
- Always validate: "Does this user own this object?"
- Modern fix ideas: Use UUIDs instead of sequential IDs, or map references through a user-scoped index.
- Traditional wisdom still holds: Don't expose internal keys. Ever. The old ways (proper authz) were right all along.

## Tools Used
- Kali Linux in VirtualBox
- Burp Suite Professional (Repeater go brrr)
- Firefox for the lab browser

Another brick in the wall. Next lab loading... stay locked in.

#WebSecurity #IDOR #PortSwiggerAcademy #BugBountyHunter #CyberSecWriteups
