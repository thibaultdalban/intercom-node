# intercom-node-v2

[![Circle CI](https://circleci.com/gh/intercom/intercom-node.png?style=shield)](https://circleci.com/gh/intercom/intercom-node)
[![npm](https://img.shields.io/npm/v/intercom-client)](https://www.npmjs.com/package/intercom-client)
![Intercom API Version](https://img.shields.io/badge/Intercom%20API%20Version-2.3-blue)
![Typescript Supported](https://img.shields.io/badge/Typescript-Supported-lightgrey)

> Official Node bindings to the [Intercom API](https://api.intercom.io/docs)

## Project Updates

### Maintenance

Current repository is a new WIP version of Node SDK, that supports latest API version (v2.3 as of 14/12/2021)

## Installation

```bash
yarn add intercom-client
```

**This client is intended for server side use only. Please use the [Intercom Javascript SDK](https://developers.intercom.com/v2.0/docs/intercom-javascript) for client-side operations.**

## Testing

```bash
yarn test
```

## Running the code locally

Compile using babel:

```bash
yarn prepublish
```

## Usage

Import Intercom:

```typescript
import { Client } from './dist/index';
```

Create a client using access tokens:

```typescript
const client = new Client({ token: 'my_token' });
```

## Request Options

This client library also supports passing in [`request` options](https://github.com/axios/axios#request-config):

```typescript
const client = new Client({ token: 'my_token' });
client.useRequestOpts({
    baseURL: 'http://local.test-server.com',
});
```

Note that certain request options (such as `json`, and certain `headers` names cannot be overriden).

### Setting the API version

We version our API (see the "Choose Version" section of the [API & Webhooks Reference](https://developers.intercom.com/intercom-api-reference/reference) for details). You can specify which version of the API to use when performing API requests using request options:

```typescript
const client = new Client({ token: 'my_token' });
client.useRequestOpts({
    headers: {
        'Intercom-Version': 2.3,
    },
});
```

## New Version

### Admins

#### [Retrieve and admin](https://developers.intercom.com/intercom-api-reference/reference/view-an-admin)

```typescript
const admin = await client.admins.find({ id: '123' });
```

#### [Set Admin away](https://developers.intercom.com/intercom-api-reference/reference/set-admin-away-mode)

```typescript
await client.admins.away({
    adminId: '123',
    enableAwayMode: true,
    enableReassignMode: false,
});
```

#### [List all activity logs](https://developers.intercom.com/intercom-api-reference/reference/view-admin-activity-logs)

```typescript
await client.admins.listAllActivityLogs({
    before: new Date('Fri, 17 Dec 2021 18:02:18 GMT');,
    after: new Date('Fri, 17 Dec 2021 18:02:18 GMT');,
});
```

#### [List all admins](https://developers.intercom.com/intercom-api-reference/reference/list-admins)

```typescript
const admins = await client.admins.list();
```

### Companies

#### [Create a company](https://developers.intercom.com/intercom-api-reference/reference/create-or-update-company)

```typescript
const company = await client.companies.create({
    createdAt: dateToUnixTimestamp(new Date()),
    companyId: '46029',
    name: 'BestCompanyInc.',
    monthlySpend: 9001,
    plan: '1. Get pizzaid',
    size: 62049,
    website: 'http://the-best.one',
    industry: 'The Best One',
    customAttributes: {},
});
```

#### [Update a company](https://developers.intercom.com/intercom-api-reference/reference/update-a-company)

```typescript
const company = await client.companies.create({
    createdAt: dateToUnixTimestamp(new Date()),
    companyId: '46029',
    name: 'BestCompanyInc.',
    monthlySpend: 9001,
    plan: '1. Get pizzaid',
    size: 62049,
    website: 'http://the-best.one',
    industry: 'The Best One',
    customAttributes: {},
});
```

#### [Retrieve a company](https://developers.intercom.com/intercom-api-reference/reference/view-a-company)

##### By id

```typescript
const company = await client.companies.find({
    companyId: 123,
});
```

##### By name

```typescript
const company = await client.companies.find({
    name: 'bruh moment inc.',
});
```

#### [Delete a company](https://developers.intercom.com/intercom-api-reference/reference/delete-a-company)

```typescript
const company = await client.companies.delete({
    id: 62049,
});
```

#### [List all companies](https://developers.intercom.com/intercom-api-reference/reference/list-companies)

##### With pagination

```typescript
const companies = await client.companies.list({
    page: 1,
    perPage: 35,
    order: Order.DESC,
});
```

##### With TagId and SegmentId

```typescript
const companies = await client.companies.list({
    tagId: '1234',
    segmentId: '4567',
});
```

#### [Scroll over companies](https://developers.intercom.com/intercom-api-reference/reference/iterating-over-all-companies)

##### Using infinite scroll

```typescript
const companies = await client.companies.scroll.each({});
```

##### Using manual scroll

```typescript
const companies = await client.companies.scroll.next({
    scrollParam: '123_soleil',
});
```

#### [Attach a contact](https://developers.intercom.com/intercom-api-reference/reference/attach-contact-to-company)

```typescript
const response = await client.companies.attachContact({
    contactId: '123',
    companyId: '234',
});
```

#### [Detach a contact](https://developers.intercom.com/intercom-api-reference/reference/detach-contact-from-company)

```typescript
const response = await client.companies.detachContact({
    contactId: '123',
    companyId: '234',
});
```

#### [List attached contacts](https://developers.intercom.com/intercom-api-reference/reference/list-company-contacts)

```typescript
const response = await client.companies.listAttachedContacts({
    companyId: '123',
    page: 1,
    perPage: 15,
});
```

#### [List attached segments](https://developers.intercom.com/intercom-api-reference/reference/list-attached-segments-1)

```typescript
const response = await client.companies.listAttachedSegments({
    companyId: '123',
});
```

### Contacts

#### [Create Contact](https://developers.intercom.com/intercom-api-reference/reference/create-contact)

##### With User Role

```typescript
const user = await client.contacts.createUser({
    externalId: '536e564f316c83104c000020',
    phone: '+48370044567',
    name: 'Niko Bellic',
    avatar: 'https://nico-from-gta-iv.com/lets_go_bowling.jpg',
    signedUpAt: 1638203719,
    lastSeenAt: 1638203720,
    ownerId: '536e564f316c83104c000021',
    isUnsubscribedFromEmails: true,
});
```

##### With Lead Role

```typescript
const lead = await client.contacts.createLead({
    phone: '+48370044567',
    name: 'Roman Bellic',
    avatar: 'https://nico-from-gta-iv.com/lets_go_bowling_yey.jpg',
    signedUpAt: 1638203719,
    lastSeenAt: 1638203720,
    ownerId: '536e564f316c83104c000021',
    isUnsubscribedFromEmails: true,
});
```

#### [Retrieve a Contact](https://developers.intercom.com/intercom-api-reference/reference/get-contact)

```typescript
const response = await client.contacts.find({ id: '123' });
```

#### [Update a Contact](https://developers.intercom.com/intercom-api-reference/reference/update-contact)

```typescript
const response = await client.contacts.update({
    id: '123',
    role: Role.USER,
    name: 'Roman The Bowling Fan',
    customAttributes: {
        callBrother: "Hey Niko, it's me – Roman. Let's go bowling!",
    },
});
```

#### [Delete a Contact](https://developers.intercom.com/intercom-api-reference/reference/delete-contact)

```typescript
const response = await client.contacts.delete({ id: '123' });
```

#### [Archive a Contact](https://developers.intercom.com/intercom-api-reference/reference/archive-a-contact)

```typescript
const response = await client.contacts.archive({ id: '123' });
```

#### [Unarchive a Contact](https://developers.intercom.com/intercom-api-reference/reference/unarchive-a-contact)

```typescript
const response = await client.contacts.unarchive({ id: '123' });
```

#### [Merge two Contacts](https://developers.intercom.com/intercom-api-reference/reference/merge-contact)

```typescript
const response = await client.contacts.mergeLeadInUser({
    leadId: '123',
    userId: '234',
});
```

#### [Search for contacts](https://developers.intercom.com/intercom-api-reference/reference/search-for-contacts)

```typescript
const response = await client.contacts.search({
    data: {
        query: {
            operator: Operators.AND,
            value: [
                {
                    operator: Operators.AND,
                    value: [
                        {
                            field: 'updated_at',
                            operator: Operators.GREATER_THAN,
                            value: 1560436650,
                        },
                        {
                            field: 'conversation_rating.rating',
                            operator: Operators.EQUALS,
                            value: 1,
                        },
                    ],
                },
                {
                    operator: Operators.OR,
                    value: [
                        {
                            field: 'updated_at',
                            operator: Operators.GREATER_THAN,
                            value: 1560436650,
                        },
                        {
                            field: 'conversation_rating.rating',
                            operator: Operators.EQUALS,
                            value: 2,
                        },
                    ],
                },
            ],
        },
        pagination: {
            per_page: 5,
            starting_after:
                'WzE2MzU4NjA2NDgwMDAsIjYxODJiNjJlNDM4YjdhM2EwMWE4YWYxNSIsMl0=',
        },
        sort: { field: 'name', order: SearchContactOrderBy.ASC },
    },
});
```

#### [List all Contacts](https://developers.intercom.com/intercom-api-reference/reference/list-contacts)

##### With cursor

```typescript
const response = await client.contacts.list({
    perPage: 5,
    startingAfter:
        'WzE2MzU3NzU4NjkwMDAsIjYxODJiNjJhMDMwZTk4OTBkZWU4NGM5YiIsMl0=',
});
```

##### Without a cursor

```typescript
const response = await client.contacts.list();
```

#### [List attached companies](https://developers.intercom.com/intercom-api-reference/reference/list-companies-of-contact)

```typescript
const response = await client.contacts.listAttachedCompanies({
    id: '123',
    perPage: 5,
    page: 1,
});
```

#### [List attached tags](https://developers.intercom.com/intercom-api-reference/reference/list-tags-of-contact)

```typescript
const response = await client.contacts.listAttachedTags({ id: '123' });
```

#### [List attached segments](https://developers.intercom.com/intercom-api-reference/reference/list-attached-segments)

```typescript
const response = await client.contacts.listAttachedSegments({ id: '123' });
```

#### [List attached email subscriptions](https://developers.intercom.com/intercom-api-reference/reference/list-attached-email-subscriptions)

```typescript
const response = await client.contacts.listAttachedEmailSubscriptions({
    id: '123',
});
```

### Conversations

#### [Create a conversation](https://developers.intercom.com/intercom-api-reference/reference/create-a-conversation)

```typescript
const response = await client.conversations.create({
    userId: '123',
    body: 'Hello darkness my old friend',
});
```

#### [Retrieve a conversation](https://developers.intercom.com/intercom-api-reference/reference/retrieve-a-conversation)

##### Normal

```typescript
const response = await client.conversations.find({
    id: '123',
});
```

##### As plain text

```typescript
const response = await client.conversations.find({
    id: '123',
    inPlainText: true,
});
```

#### [Update a conversation](https://developers.intercom.com/intercom-api-reference/reference/update-a-conversation)

```typescript
const response = await client.conversations.update({
    id,
    markRead: true,
    customAttributes: {
        anything: 'you want',
    },
});
```

#### [Reply to a conversation](https://developers.intercom.com/intercom-api-reference/reference/reply-to-a-conversation)

##### By id

###### As user

```typescript
const response = await client.conversations.replyByIdAsUser({
    id: '098',
    body: 'blablbalba',
    intercomUserId: '123',
    attachmentUrls: '345',
});
```

###### As admin

```typescript
const response = await client.conversations.replyByIdAsAdmin({
    id: '098',
    adminId: '458',
    messageType: ReplyToConversationMessageType.NOTE,
    body: '<b>Bee C</b>',
    attachmentUrls: ['https://site.org/bebra.jpg'],
});
```

##### By last conversation

###### As user

```typescript
const response = await client.conversations.replyByLastAsUser({
    body: 'blablbalba',
    intercomUserId: '123',
    attachmentUrls: '345',
});
```

###### As admin

```typescript
const response = await client.conversations.replyByLastAsAdmin({
    adminId: '458',
    messageType: ReplyToConversationMessageType.NOTE,
    body: '<b>Bee C</b>',
    attachmentUrls: ['https://site.org/bebra.jpg'],
});
```

#### [Assign a conversation](https://developers.intercom.com/intercom-api-reference/reference/assign-a-conversation)

##### As team without assignment rules

```typescript
const response = await client.conversations.assign({
    id: '123',
    type: AssignToConversationUserType.TEAM,
    adminId: '456',
    assigneeId: '789',
    body: '<b>blablbalba</b>',
});
```

##### As team with assignment rules

```typescript
const response = await client.conversations.assign({
    id: '123',
    withRunningAssignmentRules: true,
});
```

#### [Snooze a conversation](https://developers.intercom.com/intercom-api-reference/reference/snooze-a-conversation)

```typescript
const response = await client.conversations.snooze({
    id: '123',
    adminId: '234',
    snoozedUntil: '1501512795',
});
```

#### [Close a conversation](https://developers.intercom.com/intercom-api-reference/reference/close-a-conversation)

```typescript
const response = await client.conversations.close({
    id: '123',
    adminId: '456',
    body: "That's it...",
});
```

#### [Open a conversation](https://developers.intercom.com/intercom-api-reference/reference/open-a-conversation)

```typescript
const response = await client.conversations.open({
    id: '123',
    adminId: '234',
});
```

#### [Attach a contact to group conversation](https://developers.intercom.com/intercom-api-reference/reference/adding-to-group-conversations-as-admin)

##### As admin, using intercomUserid

```typescript
const response = await client.conversations.attachContactAsAdmin({
    id: '123',
    adminId: '234',
    customer: {
        intercomUserId: '456',
    },
});
```

##### As contact, using intercomUserid

```typescript
const response = await client.conversations.attachContactAsAdmin({
    id: '123',
    userId: '234',
    customer: {
        intercomUserId: '456',
    },
});
```

#### [Delete a contact from group conversation as admin](https://developers.intercom.com/intercom-api-reference/reference/deleting-from-group-conversations)

```typescript
const response = await client.conversations.detachContactAsAdmin({
    conversationId: '123',
    contactId: '456',
    adminId: '789',
});
```

#### [Search for conversations](https://developers.intercom.com/intercom-api-reference/reference/search-for-conversations)

```typescript
const response = await client.conversations.search({
    data: {
        query: {
            operator: Operators.AND,
            value: [
                {
                    operator: Operators.AND,
                    value: [
                        {
                            field: 'updated_at',
                            operator: Operators.GREATER_THAN,
                            value: 1560436650,
                        },
                        {
                            field: 'conversation_rating.rating',
                            operator: Operators.EQUALS,
                            value: 1,
                        },
                    ],
                },
                {
                    operator: Operators.OR,
                    value: [
                        {
                            field: 'updated_at',
                            operator: Operators.GREATER_THAN,
                            value: 1560436650,
                        },
                        {
                            field: 'conversation_rating.rating',
                            operator: Operators.EQUALS,
                            value: 2,
                        },
                    ],
                },
            ],
        },
        pagination: {
            per_page: 5,
            starting_after:
                'WzE2MzU4NjA2NDgwMDAsIjYxODJiNjJlNDM4YjdhM2EwMWE4YWYxNSIsMl0=',
        },
        sort: {
            field: 'name',
            order: SearchConversationOrderBy.DESC,
        },
    },
});
```

#### [List all conversations](https://developers.intercom.com/intercom-api-reference/reference/list-conversations)

```typescript
const response = await client.conversations.list({
    query: {
        order: Order.DESC,
        sort: SortBy.UpdatedAt,
        page: 1,
        perPage: 10,
    },
});
```

#### [Redact a conversation](https://developers.intercom.com/intercom-api-reference/reference/redact-a-conversation-part)

```typescript
const response = await client.conversations.redactConversationPart({
    type: RedactConversationPartType.CONVERSATION_PART,
    conversationId: '123',
    conversationPartId: '456',
});
```

### Data Attributes

#### [Create Data Attribute](https://developers.intercom.com/intercom-api-reference/reference/create-data-attributes)

```typescript
const response = await client.dataAttributes.create({
    name: 'list_cda',
    model: ModelType.CONTACT,
    dataType: DataType.STRING,
    description: 'You are either alive or dead',
    options: [{ value: 'alive' }, { value: 'dead' }],
});
```

#### [Update Data Attribute](https://developers.intercom.com/intercom-api-reference/reference/update-data-attributes)

```typescript
const response = await client.dataAttributes.update({
    id: '123',
    description: 'You are either alive or dead',
    options: [{ value: 'alive' }, { value: 'dead' }],
    archived: true,
});
```

#### [List all Data Attributes](https://developers.intercom.com/intercom-api-reference/reference/list-data-attributes)

```typescript
const response = await client.dataAttributes.list({
    model: ModelType.CONTACT,
    includeArchived: true,
});
```

### Events

#### [Submit a data event](https://developers.intercom.com/intercom-api-reference/reference/list-data-attributes)

```typescript
const response = await client.events.create({
    eventName: 'placed-order',
    createdAt: 1389913941,
    userId: 'f4ca124298',
    metadata: {
        order_date: 1392036272,
        stripe_invoice: 'inv_3434343434',
        order_number: {
            value: '3434-3434',
            url: 'https://example.org/orders/3434-3434',
        },
        price: {
            currency: 'usd',
            amount: 2999,
        },
    },
});
```

#### [List all data events](https://developers.intercom.com/intercom-api-reference/reference/list-user-events)

```typescript
const response = await client.events.listBy({
    userId: '1234',
    perPage: 2,
    summary: true,
    email: 'i_love_memes@gmail.com',
});
```

### Segments

#### Placeholder

```typescript

```

### Tags

#### Placeholder

```typescript

```

### Teams

#### Placeholder

```typescript

```

## Old Version (to be removed progressively)

### Users

```node
// Create a user
client.users.create(
    {
        email: 'jayne@serenity.io',
        custom_attributes: {
            foo: 'bar',
        },
    },
    callback
);

// Update a user
client.users.update(
    {
        email: 'jayne@serenity.io',
        custom_attributes: {
            foo: 'bar',
        },
    },
    callback
);
```

```node
// Create/update a user with custom attributes
client.users.create(
    { email: 'jayne@serenity.io', custom_attributes: { invited_friend: true } },
    callback
);
```

```node
// List users
client.users.list(callback);
```

```node
// List users by tag or segment
client.users.listBy({ tag_id: 'haven' }, callback);
```

```node
// Scroll through users list
client.users.scroll.each({}, function (res) {
    // if you return a promise from your callback, the client will only scroll
    // after this promise has resolved
    new Bluebird((resolve) => {
        setTimeout(() => {
            console.log(res.body.users.length);
            // Your custom logic
            resolve();
        }, 500);
    });
});
```

```node
// Find user by id
client.users.find({ id: '55b9eaf' }, callback);

// Find user by user_id
client.users.find({ user_id: 'foobar' }, callback);

// Find user by email
client.users.find({ email: 'jayne@serenity.io' }, callback);
```

```node
// Archive user by id (https://developers.intercom.com/intercom-api-reference/reference#archive-a-user)
client.users.archive({ id: '1234' }, callback);
```

```node
// Permanently delete a user by id (https://developers.intercom.com/intercom-api-reference/reference#delete-users)
const intercomUserId = '123';
client.users.requestPermanentDeletion(intercomUserId, callback);
```

```node
// Permanently delete a user by id in params
client.users.requestPermanentDeletionByParams({ id: '55b9eaf' }, callback);

// Permanently delete a user by user_id
client.users.requestPermanentDeletionByParams({ user_id: 'foobar' }, callback);

// Permanently delete a user by email
client.users.requestPermanentDeletionByParams(
    { email: 'jayne@serenity.io' },
    callback
);
```

### Leads

```node
// Create a contact
client.leads.create(function (r) {
    console.log(r);
});

// Create a contact with attributes
client.leads.create({ email: 'jayne@serenity.io' }, function (r) {
    console.log(r);
});
```

```node
// Update a contact by id
client.leads.update({ id: '5435345', email: 'wash@serenity.io' }, callback);
```

```node
// List contacts
client.leads.list(callback);
```

```node
// Scroll through contacts list
client.leads.scroll.each({}, function (res) {
    // if you return a promise from your callback, the client will only scroll
    // after this promise has resolved
    new Bluebird((resolve) => {
        setTimeout(() => {
            console.log(res.body.contacts.length);
            // Your custom logic
            resolve();
        }, 500);
    });
});
```

```node
// List contacts by email
client.leads.listBy({ email: 'wash@serenity.io' }, callback);
```

```node
// Find contact by id
client.leads.find({ id: '5342423' }, callback);
```

```node
// Delete contact by id
client.leads.delete({ id: '5342423' }, callback);
```

```node
// Convert Leads into Users
var conversion = {
    contact: { user_id: '1234-5678-9876' },
    user: { email: 'mal@serenity.io' },
};
client.leads.convert(conversion, callback);
```

### Customers

```node
// Search for customers
client.customers.search(
    {
        query: { field: 'name', operator: '=', name: 'Alice' },
        sort: { field: 'name', order: 'ascending' },
        pagination: { per_page: 10 },
    },
    callback
);
```

### Visitors

```node
// Update a visitor by id
client.visitors.update({ id: '5435345', email: 'wash@serenity.io' }, callback);
```

```node
// Find visitor by id or user_id
client.visitors.find({ id: '5342423' }, callback);
client.visitors.find(
    { user_id: '5b868511-ca3b-4eac-8d26-cfd82a83ac76' },
    callback
);
```

```node
// Delete visitor by id
client.visitors.delete({ id: '5342423' }, callback);
```

```node
// Convert visitors into Users
var conversion = {
    visitor: { user_id: '1234-5678-9876' },
    user: { email: 'mal@serenity.io' },
    type: 'user',
};
client.visitors.convert(conversion, callback);
```

```node
// Convert visitors into Lead
var conversion = {
    visitor: { user_id: '1234-5678-9876' },
    type: 'lead',
};
client.visitors.convert(conversion, callback);
```

### Companies

```node
// Create/update a company
client.companies.create({ company_id: '1234', name: 'serenity' }, function (r) {
    console.log(r);
});
```

```node
// List companies
client.companies.list(callback);
```

```node
// List companies by tag or segment
client.companies.listBy({ tag_id: 'haven' }, callback);
```

```node
// Scroll through companies list
client.companies.scroll.each({}, function (res) {
    // if you return a promise from your callback, the client will only scroll
    // after this promise has resolved
    new Bluebird((resolve) => {
        setTimeout(() => {
            console.log(res.body.companies.length);
            // Your custom logic
            resolve();
        }, 500);
    });
});
```

```node
// Find company by id
client.companies.find({ id: '1234' }, callback);
```

```node
// List company users by id or company_id
client.companies.listUsers({ id: '1234' }, callback);
client.companies.listUsers({ company_id: '1234' }, callback);
```

### Events

Note: events will work when identified by 'email'. The `event_name` and `created_at` params are both required. Either `user_id` OR `email` is required.

```node
// Create a event
client.events.create(
    {
        event_name: 'Foo',
        created_at: 1439826340,
        user_id: 'bar',
        metadata: { type: 'baz' },
    },
    function (d) {
        console.log(d);
    }
);
```

```node
// List events by user
client.events.listBy(
    {
        type: 'user',
        user_id: 'bar',
    },
    callback
);
```

### Counts

```node
client.counts.appCounts(callback);

client.counts.conversationCounts(callback);

client.counts.conversationAdminCounts(callback);

client.counts.userTagCounts(callback);

client.counts.userSegmentCounts(callback);

client.counts.companyTagCounts(callback);

client.counts.companySegmentCounts(callback);

client.counts.companyUserCounts(callback);
```

### Admins

```node
// List admins
client.admins.list(callback);
```

```node
// Find current admin (only works with OAuth tokens)
client.admins.me(callback);
```

```node
// Find admin by ID
client.admins.find('123456789', callback);
```

```node
// Update admin away mode and reassign settings
client.admins.away(
    '123456789',
    { away_mode_enabled: true, away_mode_reassign: false },
    callback
);
```

### Tags

```node
// Create a tag
client.tags.create({ name: 'haven' }, callback);
```

```node
// Tag a user by id
client.tags.tag({ name: 'haven', users: [{ id: '54645654' }] }, callback);
```

```node
// Tag a company by id
client.tags.tag({ name: 'haven', companies: [{ id: '54645654' }] }, callback);
```

```node
// Untag a user by id
client.tags.untag({ name: 'haven', users: [{ id: '5345342' }] }, callback);
```

```node
// List tags
client.tags.list(callback);
```

```node
// Delete a tag by id
client.tags.delete({ id: '130963' }, callback);
```

### Segments

```node
// List segments
client.segments.list(callback);
```

```node
// Find segment by id
client.segments.find({ id: '55719a4a' }, callback);
```

### Messages

```node
// Admin initiated messages:
// Sending an email to a User
var message = {
    message_type: 'email',
    subject: 'Hey',
    body: 'Ponies, cute small horses or something more sinister?',
    template: 'plain',
    from: {
        type: 'admin',
        id: '21599',
    },
    to: {
        type: 'user',
        id: '55c1ce1def857c31f80001af',
    },
};

client.messages.create(message, callback);
```

```node
// Creating a user-initiated message:
var message = {
    from: {
        type: 'user',
        id: '55c1ce1def857c31f80001af',
    },
    body: 'Howdy',
};

client.messages.create(message, callback);
```

### Conversations

Listing conversations ([documentation](https://developers.intercom.com/intercom-api-reference/reference#list-conversations)):

```node
client.conversations.list({ type: 'admin', admin_id: 21599 }, callback);
```

```node
// Fetch a conversation
client.conversations.find({ id: '1062682196' }, callback);
```

```node
// Reply to a conversation
var reply = {
    id: '1039067180',
    intercom_user_id: '55b26822ce97179e52001334',
    body: 'Some reply :)',
    type: 'user',
    message_type: 'comment',
};

client.conversations.reply(reply, callback);

// Reply to a conversation with attachments
var reply = {
    id: '1039067180',
    intercom_user_id: '55b26822ce97179e52001334',
    body: 'Some reply :)',
    type: 'user',
    message_type: 'comment',
    attachment_urls: ['http://www.example.com/myattachment.jpg'],
};

client.conversations.reply(reply, callback);
```

```node
// Assign a conversation to an admin
var assignment = {
    id: '13879167940',
    type: 'admin',
    admin_id: '1309092',
    assignee_id: '1723471',
    message_type: 'assignment',
};

client.conversations.reply(assignment, callback);

// Assign a conversation to unassigned
var assignment = {
    id: '13879167940',
    type: 'admin',
    admin_id: '1309092',
    assignee_id: '0',
    message_type: 'assignment',
};

client.conversations.reply(assignment, callback);
```

```node
// Mark a conversation as read
client.conversations.markAsRead({ id: '1039067180' }, callback);
```

### Notes

```node
// Create a note
var note = {
    admin_id: 21599,
    body: 'Hello notes!',
    user: {
        id: '55b26822ce97179e52001334',
    },
};

client.notes.create(note, callback);
```

```node
// List notes by user
client.notes.list({ email: 'bob@intercom.io' }, callback);
```

```node
//Fetch a note
client.notes.find({ id: '3342887' }, callback);
```

### Pagination

When listing, the Intercom API may return a pagination object:

```json
{
    "pages": {
        "next": "..."
    }
}
```

You can grab the next page of results using the client:

```node
client.nextPage(response.pages, callback);
```

### Identity verification

`intercom-node` provides a helper for using [identity verification](https://docs.intercom.com/configure-intercom-for-your-product-or-site/staying-secure/enable-identity-verification-on-your-web-product):

```node
import { IdentityVerification } from 'intercom-client';

IdentityVerification.userHash({
    secretKey: 's3cre7',
    identifier: 'jayne@serenity.io',
});
```

## License

Apache-2.0

## Pull Requests

-   **Add tests!** Your patch won't be accepted if it doesn't have tests.

-   **Document any change in behaviour**. Make sure the README and any other
    relevant documentation are kept up-to-date.

-   **Create topic branches**. Don't ask us to pull from your master branch.

-   **One pull request per feature**. If you want to do more than one thing, send
    multiple pull requests.

-   **Send coherent history**. Make sure each individual commit in your pull
    request is meaningful. If you had to make multiple intermediate commits while
    developing, please squash them before sending them to us.
