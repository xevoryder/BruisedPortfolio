# Troubleshooting Method

## Core Method

1. Define expected behavior.
2. Capture actual behavior.
3. Identify the likely layer:
   - User/input
   - Client/browser
   - Application
   - Config file
   - Service/process
   - OS permissions
   - Network/routing
   - External dependency
4. Test one variable at a time.
5. Apply the smallest reasonable fix.
6. Validate the result.
7. Document what changed.

## Operator Log Format

Every strong technical note should answer:

- What was the objective?
- What happened?
- What did I assume at first?
- What was actually wrong?
- What did I change?
- How did I validate it?
- What did I learn?

## Why This Matters

Good troubleshooting is not guessing faster. It is reducing uncertainty in a controlled way.
