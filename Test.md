```mermaid
graph LR
  A[needs-triage] --> B{needs-info} [Transition to needs-info if more information is requested]
  B --> A{Go back to triage on activity}
  B --> C{Mark issue as stale after timeout} [timeout: 30 days]
  C --> D{Close if stale for too long} [timeout: 30 days]
  A --> E{help-wanted}
  E --> F{Move into in-progress state} [/in-progress command]
  E --> C{Mark as stale after a timeout if nobody picks it up} [timeout: 90 days]
  C --> E{Remove stale on activity}
  E --> D{Close if stale for too long and still nobody picks it up} [timeout: 90 days]
  F --> C{Mark as stale after a timeout, then move back to help-wanted if still stale} [timeout: 30 days]
  C --> F{Remove stale on activity}
  C --> E{Move back to help-wanted if stale for too long} [timeout: 30 days]
```mermaid
This will render the state diagram directly within your GitHub repository.
