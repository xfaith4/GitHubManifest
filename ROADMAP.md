# GitHub Repository Manifest - Enhancement Roadmap

This roadmap outlines the planned enhancements to improve the dashboard's usability and reporting capabilities, with a focus on displaying open issues, projects, and especially pull requests.

---

## 📋 Executive Summary

The GitHub Repository Manifest dashboard currently provides an excellent overview of repositories with features like search, sort, filtering, and expandable READMEs. This roadmap proposes enhancements to transform it into a comprehensive project management dashboard by adding support for:

- **Pull Requests** (Highest Priority)
- **Issues** tracking
- **Projects** integration
- Enhanced reporting and analytics

---

## 🎯 Phase 1: Pull Requests Integration (Highest Priority)

### Objective
Display pull request information for each repository to help users track ongoing development work.

### Planned Features

#### 1.1 Per-Repository PR Count Badge
- Show number of open PRs on each repository card
- Visual indicator (badge) for quick scanning
- Click to expand PR list

#### 1.2 Pull Request List View
- Display list of open PRs per repository
- Show PR title, author, creation date, and status
- Visual indicators for:
  - Draft PRs
  - PRs with requested changes
  - PRs ready for merge
  - PR review status

#### 1.3 Pull Request Quick Actions
- Link to PR on GitHub
- Show merge status and CI check results
- Display assigned reviewers

### Implementation Notes
```graphql
# Add to existing GraphQL query in fetchRepos()
{
  user(login: "${username}") {
    repositories(first: 100) {
      nodes {
        name
        # ... existing fields ...
        pullRequests(first: 10, states: OPEN) {
          nodes {
            title
            number
            state
            isDraft
            author { login }
            createdAt
            mergeable
            reviewDecision
          }
          totalCount
        }
      }
    }
  }
}
```

### API Endpoints
- **GraphQL**: `pullRequests` field on Repository type
- **REST fallback**: `GET /repos/{owner}/{repo}/pulls`

---

## 🐛 Phase 2: Issues Integration

### Objective
Integrate issue tracking to show open issues and their status for each repository.

### Planned Features

#### 2.1 Issue Count Badge
- Display count of open issues on repository cards
- Color-coded indicators based on priority/labels

#### 2.2 Issues List View
- Expandable section showing open issues
- Display issue title, labels, assignees, and creation date
- Filter by label (bug, enhancement, etc.)

#### 2.3 Issue Statistics
- Show issues opened vs closed over time
- Display average issue resolution time
- Highlight stale issues (no activity for X days)

### Implementation Notes
```graphql
# Add to existing GraphQL query in fetchRepos()
{
  user(login: "${username}") {
    repositories(first: 100) {
      nodes {
        name
        # ... existing fields ...
        issues(first: 10, states: OPEN) {
          nodes {
            title
            number
            state
            labels(first: 5) { nodes { name color } }
            assignees(first: 3) { nodes { login } }
            createdAt
            updatedAt
          }
          totalCount
        }
      }
    }
  }
}
```

### API Endpoints
- **GraphQL**: `issues` field on Repository type
- **REST fallback**: `GET /repos/{owner}/{repo}/issues`

---

## 📊 Phase 3: Projects Integration

### Objective
Display GitHub Projects associated with repositories to track project progress and kanban boards.

### Planned Features

#### 3.1 Project Overview
- List projects associated with each repository
- Show project name and description
- Display progress bar based on item completion

#### 3.2 Project Statistics
- Show number of items: To Do, In Progress, Done
- Quick links to project board on GitHub

#### 3.3 Cross-Repository Project View
- Dedicated section showing all user's projects
- Aggregate view across multiple repositories

### Implementation Notes
```javascript
// GraphQL query for Projects (v2)
projectsV2(first: 5) {
  nodes {
    title
    shortDescription
    url
    items(first: 100) {
      totalCount
      nodes {
        fieldValues(first: 5) {
          nodes {
            ... on ProjectV2ItemFieldSingleSelectValue {
              name
            }
          }
        }
      }
    }
  }
}
```

### API Endpoints
- **GraphQL**: `projectsV2` field (Projects v2 API)
- Note: Projects v2 requires specific GraphQL queries

---

## 🎨 Phase 4: UI/UX Improvements

### Objective
Enhance the user interface to accommodate new data while maintaining usability.

### Planned Features

#### 4.1 Dashboard Navigation Tabs
- Add tabs for: Repositories | Pull Requests | Issues | Projects
- Allow users to focus on specific areas

#### 4.2 Enhanced Repository Cards
- Redesign cards to show summary metrics:
  - ⭐ Stars | 🍴 Forks | 🔀 Open PRs | 🐛 Open Issues
- Expandable sections for PRs, Issues, README

#### 4.3 Dashboard Summary Header
- Show totals across all repositories:
  - Total open PRs
  - Total open issues
  - Active projects count

#### 4.4 Color-Coded Status Indicators
- Green: PRs ready to merge / Issues closed
- Yellow: PRs with requested changes / Stale issues
- Red: CI failures / Critical bugs

#### 4.5 Dark/Light Theme Toggle
- Explicit toggle (currently uses system preference)
- Persist user preference in localStorage

---

## 📈 Phase 5: Advanced Reporting & Analytics

### Objective
Provide insights and analytics to help users understand their GitHub activity.

### Planned Features

#### 5.1 Activity Timeline
- Show recent activity across all repositories
- Include commits, PRs, issues, and releases

#### 5.2 Contribution Metrics
- PR merge rate
- Average PR review time
- Issue resolution time

#### 5.3 Repository Health Score
- Composite score based on:
  - PR backlog
  - Issue backlog
  - Documentation completeness
  - Last commit activity

#### 5.4 Export Capabilities
- Export dashboard data as JSON/CSV
- Generate summary reports

---

## 🗓️ Implementation Timeline

| Phase | Feature | Priority | Estimated Effort |
|-------|---------|----------|------------------|
| 1 | Pull Requests Integration | 🔴 High | 2-3 weeks |
| 2 | Issues Integration | 🟠 Medium-High | 1-2 weeks |
| 3 | Projects Integration | 🟡 Medium | 2-3 weeks |
| 4 | UI/UX Improvements | 🟢 Medium | 2-4 weeks |
| 5 | Advanced Reporting | 🔵 Low | 3-4 weeks |

---

## 🔧 Technical Considerations

### Authentication
- Full features require a GitHub Personal Access Token with appropriate scopes:
  - `repo` - for private repository access
  - `read:project` - for projects access
  - `read:user` - for user information

### Rate Limiting
- GraphQL API: 5000 points per hour (authenticated)
- REST API: 5000 requests per hour (authenticated)
- Consider caching strategies for frequently accessed data

### Browser Compatibility
- Maintain support for modern browsers (Chrome, Firefox, Safari, Edge)
- No build step required - keep pure vanilla JS approach

### Error Handling
- Graceful degradation when API limits are reached
- Clear error messages for authentication issues
- Offline indicator when network is unavailable

---

## 📝 Open Questions & Future Considerations

1. **Notifications**: Should the dashboard show GitHub notifications?
2. **Milestones**: Include milestone tracking alongside issues?
3. **Releases**: Show recent releases and deployment status?
4. **Collaboration**: Multi-user view for team dashboards?
5. **Customization**: Allow users to configure visible sections?

---

## 🤝 Contributing

Contributions are welcome! If you'd like to help implement any of these features:

1. Open an issue to discuss the feature
2. Fork the repository
3. Create a feature branch
4. Submit a pull request

---

## 📄 References

- [GitHub GraphQL API Documentation](https://docs.github.com/en/graphql)
- [GitHub REST API Documentation](https://docs.github.com/en/rest)
- [GitHub Projects API](https://docs.github.com/en/issues/planning-and-tracking-with-projects)

---

*Last Updated: December 2024*
