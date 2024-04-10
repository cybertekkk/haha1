import com.atlassian.bitbucket.user.UserAdminService
import com.atlassian.bitbucket.user.ApplicationUser
import com.atlassian.bitbucket.license.LicenseService
import com.atlassian.sal.api.component.ComponentLocator

// Get necessary Bitbucket services
def userAdminService = ComponentLocator.getComponent(UserAdminService)
def licenseService = ComponentLocator.getComponent(LicenseService)

// Retrieve all users
def allUsers = userAdminService.findAllUsers()

// Initialize list to store licensed users' details
def licensedUsersDetails = []

// Iterate over all users and fetch details for licensed users
allUsers.each { user ->
    if (licenseService.isUserLicensed(user)) {
        def userDetails = userAdminService.getUserDetails(user)
        licensedUsersDetails.add(userDetails)
    }
}

// Output user details
licensedUsersDetails.each { userDetails ->
    println("Username: ${userDetails.name}")
    println("Email: ${userDetails.emailAddress}")
    println("Display Name: ${userDetails.displayName}")
    println("Active: ${userDetails.isActive()}")
    println(" ")
}


////


import com.atlassian.bitbucket.user.UserAdminService
import com.atlassian.sal.api.component.ComponentLocator

// Get necessary Bitbucket services
def userAdminService = ComponentLocator.getComponent(UserAdminService)

// Retrieve all users
def allUsers = userAdminService.findAllUsers()

// Initialize list to store user details
def userDetailsList = []

// Iterate over all users and fetch details
allUsers.each { user ->
    def userDetails = userAdminService.getUserDetails(user)
    userDetailsList.add(userDetails)
}

// Output user details
userDetailsList.each { userDetails ->
    println("User ID: ${userDetails.id}")
    println("Email: ${userDetails.emailAddress}")
    println("Last Login Date: ${userDetails.lastAuthenticationTimestamp}")
    println(" ")
}



/////


import com.atlassian.bitbucket.user.UserService
import com.atlassian.bitbucket.user.ApplicationUser
import com.atlassian.sal.api.component.ComponentLocator

// Get necessary Bitbucket services
def userService = ComponentLocator.getComponent(UserService)

// Retrieve all users
def allUsers = userService.getAllUsers()

// Initialize list to store user details
def userDetailsList = []

// Iterate over all users and fetch details
allUsers.each { user ->
    def userDetails = userService.getUserDetails(user)
    userDetailsList.add(userDetails)
}

// Output user details
userDetailsList.each { userDetails ->
    println("User ID: ${userDetails.id}")
    println("Email: ${userDetails.emailAddress}")
    println("Last Login Date: ${userDetails.lastAuthenticationTimestamp}")
    println(" ")
}


////////





