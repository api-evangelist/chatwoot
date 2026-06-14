# Chatwoot GraphQL Schema

## Overview

This is a conceptual GraphQL schema for Chatwoot, the open-source customer support and omni-channel messaging platform. Chatwoot exposes a REST API; this schema models the same domain surface in GraphQL terms, providing a typed, queryable representation of all major Chatwoot resources.

Source: https://www.chatwoot.com/developers/api/
GitHub: https://github.com/chatwoot/chatwoot

---

## Schema File

`chatwoot-schema.graphql`

---

## Type Summary

### Scalars
- `DateTime` — ISO 8601 timestamp
- `JSON` — arbitrary JSON object or array

### Enums (9)

| Enum | Values |
|---|---|
| `InboxType` | EMAIL, WEBCHAT, FACEBOOK, TWITTER, WHATSAPP, TELEGRAM, SMS, API, LINE, INSTAGRAM |
| `ConversationStatus` | OPEN, RESOLVED, PENDING, SNOOZED |
| `MessageType` | INCOMING, OUTGOING, ACTIVITY, TEMPLATE |
| `MessageContentType` | TEXT, INPUT_SELECT, INPUT_EMAIL, INPUT_CSAT, CARDS, FORM, ARTICLE, STICKER, AUDIO, VIDEO, IMAGE, FILE |
| `AgentRole` | AGENT, ADMINISTRATOR |
| `AttachmentFileType` | IMAGE, AUDIO, VIDEO, FILE, LOCATION, FALLBACK |
| `TemplateCategory` | AUTHENTICATION, MARKETING, UTILITY |
| `CustomAttributeAttributeModel` | CONTACT, CONVERSATION |
| `CustomAttributeAttributeDisplayType` | TEXT, NUMBER, CURRENCY, PERCENT, LINK, DATE, LIST, CHECKBOX |
| `WebhookEventName` | CONVERSATION_CREATED, CONVERSATION_UPDATED, CONVERSATION_STATUS_CHANGED, MESSAGE_CREATED, MESSAGE_UPDATED, CONTACT_CREATED, CONTACT_UPDATED, WEBWIDGET_TRIGGERED |
| `ReportMetricType` | ACCOUNT_SUMMARY, OVERVIEW, INBOX_SUMMARY, AGENT_SUMMARY, TEAM_SUMMARY, LABEL_SUMMARY |

### Object Types

#### Account Domain
- `Account` — top-level account resource with nested inboxes, agents, teams, labels, contacts, conversations, reports, webhooks, and canned responses
- `AccountDetails` — flattened account properties view
- `AccountAvatar` — avatar and thumbnail URLs
- `AccountFeatures` — feature flags (agent management, custom attributes, reports, API channel)

#### Inbox Domain
- `Inbox` — generic inbox with channel type, greeting, working hours, and agent assignments
- `InboxDetails` — interface implemented by all inbox channel types
- `EmailInbox` — email channel inbox
- `APIInbox` — API channel inbox with webhook URL
- `WebchatInbox` — live chat widget inbox with branding fields
- `FacebookPage` — Facebook Messenger channel inbox
- `TwitterInbox` — Twitter/X DM channel inbox
- `WhatsAppInbox` — WhatsApp Business inbox with phone number and provider
- `TelegramInbox` — Telegram bot inbox
- `SMSInbox` — SMS channel inbox

#### Contact Domain
- `Contact` — full contact with email, phone, location, company, labels, notes, and conversations
- `ContactDetails` — flattened contact properties
- `ContactEmail` — individual email address entry with primary flag
- `ContactPhone` — individual phone number entry with primary flag
- `ContactLabel` — label applied to a contact
- `ContactNote` — free-text note on a contact
- `ContactCompany` — company association
- `ContactConnection` — paginated contact list

#### Conversation Domain
- `Conversation` — conversation with status, assignee, team, labels, messages, and custom attributes
- `ConversationDetails` — flattened conversation properties
- `ConversationMeta` — aggregate counts (all, mine, unassigned, unresolved)
- `ConversationLabel` — label applied to a conversation
- `ConversationConnection` — paginated conversation list

#### Message Domain
- `Message` — a single message with type, content type, attachments, and sender
- `MessageDetails` — flattened message properties and channel
- `MessageContent` — typed content payload
- `ChatMessage` — inbound or outbound chat message
- `AgentMessage` — message sent by a human agent
- `BotMessage` — message sent by an agent bot
- `ActivityMessage` — system activity event in a conversation thread
- `Email` — email message with subject, headers, body, and attachments

#### Attachment Domain
- `Attachment` — file, image, audio, video, or location attachment on a message

#### Agent Domain
- `Agent` — agent with role, availability, team memberships, and inbox assignments
- `AgentDetails` — flattened agent properties
- `Assignment` — assignment of a conversation to an agent or team

#### Team Domain
- `Team` — team with agents
- `TeamDetails` — flattened team properties

#### Label Domain
- `Label` — account-level label with color and sidebar visibility
- `LabelDetails` — flattened label properties

#### Template Domain
- `Template` — WhatsApp message template with category, language, and components
- `TemplateDetails` — flattened template properties
- `TemplateComponent` — header, body, footer, or button component
- `TemplateButton` — CTA or quick-reply button on a template

#### Automation Domain
- `CannedResponse` — saved reply shortcut
- `CustomAttribute` — account-level custom attribute definition
- `CustomAttributeKey` — key metadata for a custom attribute

#### Reporting Domain
- `Report` — report by type (account, agent, inbox, team, label)
- `ReportSummary` — aggregate metrics: conversation counts, response times, message counts
- `ReportMetric` — time-series metric data point
- `ConversationReport` — per-conversation timing metrics

#### Webhook Domain
- `Webhook` — registered webhook endpoint with event subscriptions
- `WebhookEvent` — individual webhook delivery event with payload and response status

#### Bot Domain
- `AgentBot` — agent bot integration with outgoing URL
- `BotEndpoint` — endpoint configuration for a bot

#### Auth Domain
- `APIKey` — API key for programmatic access
- `Token` — access token response

#### Shared
- `PaginationMeta` — pagination envelope (currentPage, totalCount, totalPages, perPage)

### Input Types (12)
- `CreateConversationInput`
- `MessageInput`
- `CreateContactInput`
- `UpdateContactInput`
- `CreateAgentInput`
- `UpdateAgentInput`
- `CreateLabelInput`
- `CreateWebhookInput`
- `UpdateWebhookInput`
- `CreateCannedResponseInput`
- `CreateTeamInput`
- `AssignConversationInput`

---

## Root Operations

### Query
Covers: account, accountDetails, inboxes, inbox, contacts, contact, contactNotes, contactConversations, conversations, conversation, conversationMessages, agents, agent, teams, team, teamAgents, labels, accountReport, conversationReport, cannedResponses, customAttributes, webhooks, webhook, templates, agentBots, agentBot.

### Mutation
Covers: createConversation, updateConversationStatus, assignConversation, addConversationLabel, removeConversationLabel, createMessage, deleteMessage, createContact, updateContact, deleteContact, createContactNote, deleteContactNote, createAgent, updateAgent, deleteAgent, createTeam, updateTeam, deleteTeam, addTeamAgents, removeTeamAgents, createLabel, updateLabel, deleteLabel, createWebhook, updateWebhook, deleteWebhook, createCannedResponse, updateCannedResponse, deleteCannedResponse, createAgentBot, updateAgentBot, deleteAgentBot.

### Subscription
Real-time events: messageCreated, conversationStatusChanged, conversationAssigned, contactCreated, contactUpdated.

---

## Notes

- Chatwoot does not currently expose a native GraphQL API. This schema is a conceptual representation of its REST API domain model, intended for tooling, code generation, and documentation purposes.
- The schema maps directly to the Chatwoot Application API, Client API, and Platform API endpoints documented at https://developers.chatwoot.com/api-reference/introduction.
- The `InboxDetails` interface allows polymorphic querying across all inbox channel types.
- The `MessageSender` union covers both `Agent` and `Contact` as message origins.
- Custom attributes use a two-type model: `CustomAttributeKey` defines the schema; `CustomAttribute` holds a value instance.
