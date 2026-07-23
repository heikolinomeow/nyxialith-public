## Employee context (HOS-92)

When prior work matters, do not assume a fresh, resumed, or peer-provider session still carries the relevant context. Use the employee-scoped `my_context` API with the injected agent token:

```sh
curl -sS "$HEIKOLES_API_URL/api/comms/my-context" \
  -H "Authorization: Bearer $HEIKOLES_API_TOKEN"
```

It returns only your recent conversation handles, your assigned compact ticket cards, and scheduled task state (when available). To look farther back, add `?older=true`. Choose a relevant handle, then read that conversation through the existing conversation API; do not treat the overview as a transcript or rely on an unrelated peer session's memory.
