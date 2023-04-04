## Build {{buildDetails.buildNumber}}
* **Branch**: {{buildDetails.sourceBranch}}
* **Tags**: {{buildDetails.tags}}
* **Completed**: {{buildDetails.finishTime}}
* **Previous Build**: {{compareBuildDetails.buildNumber}}

## Associated Pull Requests ({{pullRequests.length}})
{{#forEach pullRequests}}
* **[{{this.pullRequestId}}]({{replace (replace this.url "_apis/git/repositories" "_git") "pullRequests" "pullRequest"}})** {{this.title}}
* Associated Work Items
{{#forEach this.associatedWorkitems}}
   {{#with (lookup_a_work_item ../../relatedWorkItems this.url)}}
    - [{{this.id}}]({{replace this.url "_apis/wit/workItems" "_workitems/edit"}}) - {{lookup this.fields 'System.Title'}}
   {{/with}}
{{/forEach}}
* Associated Commits (this includes commits on the PR source branch not associated directly with the build)
{{#forEach this.associatedCommits}}
    - [{{this.commitId}}]({{this.remoteUrl}}) -  {{this.comment}}
{{/forEach}}
{{/forEach}}

# Global list of WI with PRs, parents and children
{{#forEach this.workItems}}
{{#if isFirst}}### WorkItems {{/if}}
*  **{{this.id}}**  {{lookup this.fields 'System.Title'}}
   - **WIT** {{lookup this.fields 'System.WorkItemType'}}
   - **Tags** {{lookup this.fields 'System.Tags'}}
   - **Assigned** {{#with (lookup this.fields 'System.AssignedTo')}} {{displayName}} {{/with}}
   - **Description** {{{lookup this.fields 'System.Description'}}}
   - **PRs**
{{#forEach this.relations}}
{{#if (contains this.attributes.name 'Pull Request')}}
{{#with (lookup_a_pullrequest ../../pullRequests  this.url)}}
      - {{this.pullRequestId}} - {{this.title}}
{{/with}}
{{/if}}
{{/forEach}}
   - **Parents**
{{#forEach this.relations}}
{{#if (contains this.attributes.name 'Parent')}}
{{#with (lookup_a_work_item ../../relatedWorkItems  this.url)}}
      - {{this.id}} - {{lookup this.fields 'System.Title'}}
      {{#forEach this.relations}}
      {{#if (contains this.attributes.name 'Parent')}}
      {{#with (lookup_a_work_item ../../../../relatedWorkItems  this.url)}}
         - {{this.id}} - {{lookup this.fields 'System.Title'}}
      {{/with}}
      {{/if}}
      {{/forEach}}
{{/with}}
{{/if}}
{{/forEach}}
   - **Children**
{{#forEach this.relations}}
{{#if (contains this.attributes.name 'Child')}}
{{#with (lookup_a_work_item ../../relatedWorkItems  this.url)}}
      - {{this.id}} - {{lookup this.fields 'System.Title'}}
{{/with}}
{{/if}}
{{/forEach}}
   - **Tested By**
{{#forEach this.relations}}
{{#if (contains this.attributes.name 'Tested By')}}
{{#with (lookup_a_work_item ../../testedByWorkItems  this.url)}}
      - {{this.id}} - {{lookup this.fields 'System.Title'}}
{{/with}}
{{/if}}
{{/forEach}}
{{/forEach}}

# Global list of CS ({{commits.length}})
{{#forEach commits}}
{{#if isFirst}}### Associated commits{{/if}}
* ** ID{{this.id}}**
   -  **Message:** {{this.message}}
   -  **Commited by:** {{this.author.displayName}}
   -  **FileCount:** {{this.changes.length}}
{{#forEach this.changes}}
      -  **File path (TFVC or TfsGit):** {{this.item.path}}
      -  **File filename (GitHub):** {{this.filename}}
{{/forEach}}
{{/forEach}}

## List of WI returned by WIQL ({{queryWorkItems.length}})
{{#forEach queryWorkItems}}
*  **{{this.id}}** {{lookup this.fields 'System.Title'}}
{{/forEach}}

## Manual Test Plans
| Run ID | Name | State | Total Tests | Passed Tests |
| --- | --- | --- | --- | --- |
{{#forEach manualTests}}
| [{{this.id}}]({{this.webAccessUrl}}) | {{this.name}} | {{this.state}} | {{this.totalTests}} | {{this.passedTests}} |
{{/forEach}}

## Global list of ConsumedArtifacts ({{consumedArtifacts.length}})
| Category | Type | Version Name | Version Id | Commits | Workitems |
|-|-|-|-|-|-|
{{#forEach consumedArtifacts}}
 |{{this.artifactCategory}} | {{this.artifactType}} | {{#if versionName}}{{versionName}}{{/if}} | {{truncate versionId 7}} | {{#if this.commits}} {{this.commits.length}} {{/if}} | {{#if this.workitems}} {{this.workitems.length}} {{/if}} |
{{/forEach}}

## Artifacts published by build ({{publishedArtifacts.length}})
| Name| Type |
|-|-|-|
{{#forEach publishedArtifacts}}
 |{{this.name}} | {{this.resource.type}} | 
{{/forEach}}
