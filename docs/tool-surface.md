# MCP tool surface

This snapshot comes from the production release of October 2026. The skill, account and feedback tools are registered by the connector itself. Every other family is fetched at request time from its product Worker, so the exact list can change without a connector release. Every tool carries MCP tool annotations (read-only, destructive, idempotent hints).

A few owner-only triage tools exist but are hidden from regular users, so they are not listed here.

## Skills catalog (core)

| Tool | Purpose |
|---|---|
| `search_skills` | Find skills, agents and prompt packs by short keywords |
| `suggest_skills` | Take a task or goal in plain words and return up to 3 fitting specialists, or say that nothing fits |
| `get_skill` | Load a skill's full instructions into the conversation, with related roles for hand-off |
| `list_collection` | Browse the catalog by collection, used to open vague or first conversations |
| `skill_status` | App-only helper that lets the search card show what has been loaded |

**MCP prompts:** `find-specialist`, `build-team`, `browse-catalog`, `switch-role`, `my-plan`, `move-to-board`. These appear as commands in the client UI.

## Account and feedback

| Tool | Purpose |
|---|---|
| `my_plan` | Show the current plan, what it includes and its status |
| `revoke_sessions` | Sign out every connected client |
| `delete_my_account` | Delete the account and its data |
| `submit_feedback` / `report_issue` | Send product feedback or a bug report from the chat |
| `my_feedback` | List the user's own feedback and its status |

## CRM board (`crm_*`, about 85 tools)

| Group | Example tools | Purpose |
|---|---|---|
| Workspaces and members | `crm_create_workspace`, `crm_switch_workspace`, `crm_invite_member`, `crm_accept_invite` | Multi-workspace, shared boards |
| Schema and templates | `crm_get_schema`, `crm_add_field`, `crm_add_stage`, `crm_apply_template`, `crm_apply_schema_ops` | Shape a board for sales, hiring, projects and other uses |
| Records and deals | `crm_create_record`, `crm_update_record`, `crm_create_deal`, `crm_move_stage`, `crm_bulk_update`, `crm_link_records` | Day-to-day pipeline work |
| Search and views | `crm_query`, `crm_search_records`, `crm_views`, `crm_today` | Find things and see what is due today |
| Notes and activity | `crm_add_note`, `crm_add_comment`, `crm_add_attachment`, `crm_history`, `crm_rollback` | Context, audit trail and undo |
| Leads and outreach | `crm_add_leads`, `crm_enrich_lead`, `crm_draft_outreach`, `crm_set_icp`, `crm_score_candidate` | Prospecting and candidate screening |
| Import, export, integrations | `crm_import_deals`, `crm_import_report`, `crm_export`, `crm_webhooks`, `crm_migration_guide` | Move data in and out |
| Analytics and settings | `crm_analytics`, `crm_get_settings`, `crm_update_settings`, `crm_rules` | Pipeline metrics and board rules |

## Website receptionist (`rx_*`, about 23 tools)

| Group | Example tools | Purpose |
|---|---|---|
| Sites and setup | `rx_create_site`, `rx_autosetup`, `rx_import_site`, `rx_get_embed_code`, `rx_check_install` | Put an AI chat receptionist on a website |
| Knowledge | `rx_set_answers`, `rx_approve_answers`, `rx_unanswered_questions`, `rx_set_hours_and_slots` | Control what it says and when the business is available |
| Inbox | `rx_list_conversations`, `rx_list_leads`, `rx_reply_lead`, `rx_list_requests`, `rx_update_request` | Follow up on visitors and booking requests |
| Testing | `rx_test_chat` | Try the receptionist from inside the chat |

## Contracts (`contract_*`, 10 tools)

| Example tools | Purpose |
|---|---|
| `contract_upload_url`, `contract_add_text`, `contract_get_text` | Add a contract |
| `contract_save_review`, `contract_get_review` | Store and read an AI review |
| `contract_set_reminder`, `contract_upcoming_renewals` | Track renewals and deadlines |

## SEO and dashboards (`seo_*`, `dash_*`, 10 tools)

| Example tools | Purpose |
|---|---|
| `seo_audit`, `seo_get_report`, `seo_list_reports` | Run and read website SEO audits |
| `seo_set_monitor`, `seo_list_monitors` | Re-check a site on a schedule |
| `dash_create`, `dash_get`, `dash_update`, `dash_list`, `dash_delete` | Build and keep simple dashboards |

## Generators (`gen_*`, 6 tools)

| Example tools | Purpose |
|---|---|
| `gen_list_tools`, `gen_run` | List and run content generators |
| `gen_save_result`, `gen_get_result`, `gen_list_results`, `gen_delete_result` | Keep generated results |

## Interactive cards (MCP Apps)

Hosts that support MCP Apps render cards for `get_skill`, `my_plan`, `search_skills`, `crm_today` and the SEO report tools. Hosts without widget support get the same information as text.
