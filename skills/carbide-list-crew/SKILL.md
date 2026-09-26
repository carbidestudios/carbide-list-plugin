---
name: carbide-list-crew
description: Help a foreman or crew member with their Carbide List work, in English or Spanish. Use when they ask what work they have at their houses, what is blocked or waiting on parts, where to buy materials, what changed today, or say that an item is finished.
---

# Carbide List for crews

A crew member, usually a foreman, connects Carbide List with the Crew link their Builder sent them. The connection sees only the houses the Builder shared with that crew.

## Start every conversation the same way

1. Call `get_connection_context`. If its `authority.authority.kind` is not `crew`, use the carbide-list-builder skill instead.
2. Read the `guide` it returns and follow it. It outranks this skill.
3. Reply in the language the person writes in. Pass `language` (`en` or `es`) to `get_punch_list` and to the tools that store text: `add_note`, `set_blocker`, `mark_ready_for_review`, and `withdraw_ready_for_review`.

Keep answers short. The person is often on a job site, on a phone.

## "What do I have today?"

1. Call `get_updates` and `get_houses`.
2. Lead with work the Builder sent back, with the reason. Then new work, then what is blocked.
3. When they pick a house, call `get_punch_list` for it and list items by floor with their trade.

## Blockers and buying parts

- When something is missing, record it with `set_blocker`: a short `waitingOn` such as "2 GFCI outlets", plus `expectedDate` if they know it.
- When it arrives, use `mark_blocker_received`. The item stays open for the work itself.
- When they ask where to buy something, work from the blockers and item descriptions. Use your own search tools, if you have them, for nearby suppliers, stock, and prices. Carbide List does not know stores or prices. Never buy anything or commit money; suggest options and let them decide.

## "It's done"

1. Find the exact item. If more than one could match, list them and ask.
2. Repeat back the house and item, and ask them to confirm.
3. Ask if they added a photo in Carbide List. This connection cannot add photos. If there is none, ask why and pass `noPhotoReason`: `not_visible`, `cannot_now`, or `other`.
4. Call `mark_ready_for_review`. The Builder still checks and approves the work.

If they reported the wrong item, `withdraw_ready_for_review` takes it back before the Builder verifies it.

## Limits

This connection cannot approve or close work, change the Builder's instructions, add or remove items, manage photos, or see houses the Builder did not share. If every call fails with an authorization error, the Builder turned the connection off or replaced the Crew link; tell the person to ask their Builder for the current link.
