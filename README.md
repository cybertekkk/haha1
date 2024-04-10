

////////////////
import com.atlassian.bitbucket.user.UserService
import com.atlassian.bitbucket.user.ApplicationUser
import com.atlassian.bitbucket.util.PageRequest
import com.atlassian.sal.api.component.ComponentLocator

// Get necessary Bitbucket services
def userService = ComponentLocator.getComponent(UserService)

// Initialize variables for pagination
final int pageSize = 100 // Adjust as necessary
int start = 0

// Initialize list to store user details
def userDetailsList = []

// Iterate over all pages of users and fetch details
while (true) {
    def pageRequest = new PageRequest(start, pageSize)
    def usersPage = userService.findUsers(pageRequest)
    def users = usersPage.getValues()

    // Check if there are no more users
    if (users.isEmpty()) {
        break
    }

    // Iterate over users on the current page and fetch details
    users.each { user ->
        def userDetails = userService.getUserDetails(user)
        userDetailsList.add(userDetails)
    }

    // Increment start index for next page
    start += pageSize
}

// Output user details
userDetailsList.each { userDetails ->
    println("User ID: ${userDetails.id}")
    println("Email: ${userDetails.emailAddress}")
    println("Last Login Date: ${userDetails.lastAuthenticationTimestamp}")
    println(" ")
}



