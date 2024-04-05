import com.atlassian.bitbucket.auth.AuthenticationManager
import com.atlassian.bitbucket.auth.AuthenticationResult
import com.atlassian.bitbucket.auth.AuthenticationStore
import com.atlassian.bitbucket.auth.AuthenticationToken

// Inject necessary services
def authenticationManager = ComponentLocator.getComponent(AuthenticationManager)
def authenticationStore = ComponentLocator.getComponent(AuthenticationStore)

// Get all users
def users = ComponentLocator.getComponent(com.atlassian.bitbucket.user.UserService).findAll()

// Store user information
def userAuthInfo = []

// Iterate through users
users.each { user ->
    // Get authentication token for the user
    AuthenticationToken token = authenticationStore.getAuthenticationToken(user.id)
    
    if (token) {
        // Get authentication result using the token
        AuthenticationResult authResult = authenticationManager.getResult(token)
        
        // Extract authenticated date
        Date authenticatedDate = authResult.getAuthenticatedDate()
        
        // Add user information along with authenticated date to the list
        userAuthInfo.add([username: user.name, email: user.emailAddress, authenticatedDate: authenticatedDate])
    }
}

return userAuthInfo

