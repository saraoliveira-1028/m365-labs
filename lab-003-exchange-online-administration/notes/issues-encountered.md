# Issue 001 – Shared Mailbox Permission Assignment Delay

## Description

While assigning permissions to a Shared Mailbox, one user did not immediately appear in the mailbox members list.

## Symptoms

- User selected during assignment.
- Send As permission appeared correctly.
- Full Access permission was not displayed in the interface.

## Troubleshooting

The permission assignment process was repeated.

## Resolution

The user appeared correctly after the second attempt.

## Root Cause

Most likely a temporary synchronization delay or an Exchange Admin Center interface inconsistency.

## Lessons Learned

Permission assignments may require time to propagate before being reflected in the administrative interface.
