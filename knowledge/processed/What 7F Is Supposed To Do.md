What 7F Is Supposed To Do

  Milestone 7F is the first real web screen.

  Right now the frontend is just a runtime/status shell: it checks API health and counts seeded instruments/playbooks. 7F replaces that with the actual
  trade-capture workspace on top of the 7E backend.

  The user-facing flow should be:

  1. Paste normal trader language into a large input.
  2. Click parse.
  3. The UI calls POST /trade-capture/parse.
  4. The UI renders three editable sections:
      - Idea: symbol, playbook, purpose, direction, horizon
      - Thesis: reasoning, evidence, risks, disconfirming signals
      - Plan: entry criteria, invalidation, targets, risk model, sizing assumptions
  5. Missing or ambiguous fields are shown clearly next to the relevant section/field.
  6. The user edits the draft directly in the browser.
  7. The user explicitly clicks save.
  8. The UI calls POST /trade-capture/save.
  9. The UI shows a saved-result summary with the generated idea/thesis/plan IDs.

  What it must not do:

  - no plan approval
  - no rule evaluation
  - no order intent
  - no position opening
  - no fills
  - no broker actions
  - no trade recommendations
  - no claim verification

  So 7F is mainly “make the workflow usable in the browser.” 7G can still remain the stricter end-to-end acceptance slice: Docker/browser/manual validation,
  persistence verification, and final workflow polish.