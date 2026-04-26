 I found two implementation decisions that materially change the shape of the issue. The repo can support either, but the plan should lock them now so
  implementation does not drift.


  Question 1/3 (3 unanswered)
  When reusing an existing imported context snapshot for a plan, position, or review, what should the system do?

  › 1. Copy snapshot (Recommended)  Create a new immutable snapshot with the same payload/source/observed_at and a new target link, preserving the
                                    original import unchanged.
    2. Mutate link                  Update the existing snapshot's target fields in place; simpler output, but weakens snapshot immutability/audit
                                    history.
    3. No attach now                Only add discovery filters in this issue; defer post-import linking/copying.
    4. None of the above            Optionally, add details in notes (tab).

  tab to add notes | enter to submit answer | ←/→ to navigate questions | esc to interrupt
  
    Question 2/3 (2 unanswered)
  Should `list-context` still require an instrument or target filter, or become a broader discovery command?

  › 1. Allow broad list (Recommended)  Permit `list-context` with any combination of filters, including only context_type/source/date ranges or no
                                       filters.
    2. Require anchor                  Keep requiring instrument or target, and make context_type/source/date ranges additional refinements only.
    3. Separate search                 Keep `list-context` strict and add a new discovery command for broad filters.
    4. None of the above               Optionally, add details in notes (tab).

  tab to add notes | enter to submit answer | ←/→ to navigate questions | esc to interrupt
  
  
  Question 3/3 (1 unanswered)
  How should date range filters behave for observed/captured timestamps?

  › 1. Inclusive ISO (Recommended)  Use inclusive `--observed-from/--observed-to` and `--captured-from/--captured-to` options parsed as ISO datetimes.
    2. Date-only days               Use date-only options that expand to whole local days, friendlier but less precise.
    3. After before                 Use exclusive `--observed-after/--observed-before` and `--captured-after/--captured-before` options.
    4. None of the above            Optionally, add details in notes (tab).

  tab to add notes | enter to submit all | ←/→ to navigate questions | esc to interrupt
  
  
  chose the recommended answer for each.
  
  • Questions 3/3 answered
  • When reusing an existing imported context snapshot for a plan, position, or review, what should the system do?
    answer: Copy snapshot (Recommended)
  • Should `list-context` still require an instrument or target filter, or become a broader discovery command?
    answer: Allow broad list (Recommended)
  • How should date range filters behave for observed/captured timestamps?
    answer: Inclusive ISO (Recommended)


