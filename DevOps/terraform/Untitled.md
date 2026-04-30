mutable deployment: update software right in the running server.
should tell more about disadvantages and caveats of this approach 



immutable deployment: Immutable infrastructure means you must create a new resource for any changes to infrastructure configuration. You do not modify the resource after creating it.

create new machine image for every software revision. Any update require new image for deployment which will create new resource to contain new change.

Modification to server such as IP address registration should be passed as parameters to a startup script defined inside machine image