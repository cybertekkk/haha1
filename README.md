import com.atlassian.bitbucket.user.UserService
import com.atlassian.bitbucket.user.ApplicationUser
import com.atlassian.bitbucket.auth.AuthenticationStore
import com.atlassian.bitbucket.auth.AuthenticationManager
import com.atlassian.bitbucket.auth.AuthenticationToken
import com.atlassian.bitbucket.auth.AuthenticationResult
import com.atlassian.sal.api.component.ComponentLocator

// Get necessary Bitbucket services
def userService = ComponentLocator.getComponent(UserService)
def authenticationManager = ComponentLocator.getComponent(AuthenticationManager)
def authenticationStore = ComponentLocator.getComponent(AuthenticationStore)

// Function to retrieve authentication result for a user
def getAuthenticationResult(ApplicationUser user) {
    AuthenticationToken token = authenticationStore.getAuthenticationToken(user.id)
    if (token) {
        return authenticationManager.getResult(token)
    }
    return null
}

// Function to format user details
def formatUserDetails(ApplicationUser user, AuthenticationResult authResult) {
    def userDetails = "Username: ${user.name}\n" +
                      "Email: ${user.emailAddress}\n"
    if (authResult) {
        userDetails += "Authenticated Date: ${authResult.getAuthenticatedDate()}\n"
    } else {
        userDetails += "Not Authenticated\n"
    }
    return userDetails
}

// Retrieve all users
def users = userService.findUsers(null, null, null, null)

// Store user details
def userDetailsList = []

// Iterate through users and fetch details
users.each { user ->
    def authResult = getAuthenticationResult(user)
    def userDetails = formatUserDetails(user, authResult)
    userDetailsList.add(userDetails)
}

// Output user details
userDetailsList.each { userDetails ->
    log.info(userDetails)
}
