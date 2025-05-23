# FullPowers.exe

{% hint style="danger" %}
**This tool should be executed as `LOCAL SERVICE` or `NETWORK SERVICE` only.**
{% endhint %}

* Use **FullPowers** when your current user **lacks critical privileges** needed for privilege escalation or lateral movement, especially:
  * **`SeImpersonatePrivilege`** (needed for token impersonation attacks)
  * Other missing privileges that block execution of certain post-exploitation techniques.
* If privilege enumeration (e.g., via `whoami /priv`) shows **important privileges missing**, FullPowers can help **restore those privileges** by creating a scheduled task that runs a new process with the required rights.

```powershell
FullPowers
```
