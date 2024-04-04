import com.atlassian.jira.component.ComponentAccessor

def issueManager = ComponentAccessor.getIssueManager()
def customFieldManager = ComponentAccessor.getCustomFieldManager()

// Change 'Epic Link' to the appropriate field ID if your Epic linkage field is different
def epicLinkField = customFieldManager.getCustomFieldObjectByName("Epic Link")

def parentCapabilityField = customFieldManager.getCustomFieldObjectByName("Parent Capability")
def storyPointsField = customFieldManager.getCustomFieldObjectByName("Story Points")

// Retrieve the current issue and its parent capability
def currentIssue = issue
def parentCapability = currentIssue.getCustomFieldValue(parentCapabilityField)

// Calculate total completed story points in the parent capability
def totalCompletedStoryPoints = 0
if (parentCapability != null) {
    def parentCapabilityIssues = issueManager.getIssueObjects(parentCapability)
    parentCapabilityIssues.each { capabilityIssue ->
        def epicLinkValue = capabilityIssue.getCustomFieldValue(epicLinkField)
        if (epicLinkValue == currentIssue.key) {
            // This issue belongs to the current Epic, add its story points to the total
            def storyPointsValue = capabilityIssue.getCustomFieldValue(storyPointsField)
            if (storyPointsValue != null && capabilityIssue.resolutionDate) {
                totalCompletedStoryPoints += storyPointsValue as Integer
            }
        }
    }
}

return totalCompletedStoryPoints
