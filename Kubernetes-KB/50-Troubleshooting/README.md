# Troubleshooting

Diagnosis and fixes for when things go wrong.

## What This Section Is For

Answer the question: "How do I fix...?" Each page walks you through diagnosing and resolving a specific problem.

## When to Use This Section

- Something isn't working
- You see an error message
- A Pod won't start, a Service can't route traffic, etc.
- You need to diagnose the root cause

## When to Use Other Sections Instead

- **Want to prevent problems** → [20-Tasks/README.md](../20-Tasks/README.md), [30-Concepts/README.md](../30-Concepts/README.md)
- **Need background theory** → [30-Concepts/README.md](../30-Concepts/README.md)
- **Need to run operations** → [60-PlatformOps/README.md](../60-PlatformOps/README.md)

## How Troubleshooting Pages Are Structured

Each page includes:
- **Symptoms** (how do you know you have this problem?)
- **Quick fix** (immediate solution if applicable)
- **Root causes** (multiple possible causes with diagnosis steps)
- **Diagnosis flowchart** (choose your path)
- **Prevention** (how to avoid this in the future)

## How to Use This Section

1. **Match symptoms:** Find the page matching what you're seeing
2. **Diagnose:** Follow the diagnostic steps to identify the cause
3. **Fix:** Apply the fix for that cause
4. **Verify:** Confirm it worked
5. **Prevent:** Follow prevention tips

## Example Topics

- [Troubleshooting - Pod Won't Start](Troubleshooting%20-%20Pod%20Won%27t%20Start.md)
- [Troubleshooting - Service Can't Reach Pod](Troubleshooting%20-%20Service%20Can%27t%20Reach%20Pod.md)

## Severity Levels

- **Common:** Frequently encountered (easy to fix)
- **Rare:** Uncommon edge cases (harder to diagnose)

## Routing Rules

From **Troubleshooting**, you can link to:
- [20-Tasks/README.md](../20-Tasks/README.md) (for fixes)
- [30-Concepts/README.md](../30-Concepts/README.md) (for background)
- [40-Reference/README.md](../40-Reference/README.md) (for field specs)

---

**Still stuck?** 
- Check [30-Concepts/README.md](../30-Concepts/README.md) for background
- Use `kubectl describe` and `kubectl logs` to gather more info
- Search the KB for your specific error message
