# Servers_Services

## INTRODUCTION

In this project, the requirement is that there is a need for a process where in the dashboard there will be a list of application servers. The serversID, name of servers, and other details will be mentioned on the
dashboard page and the data may be displayed in the tabular form(optional). While clicking on a particular server a new page may open where the services of a particular server will open(for a particular server which is required to open). The services of that particular server(which is requested) will be listed in tabular format. Now there will be three buttons Start, Stop, Restart which when clicked will open those particular services.


## API Endpoints

In here, the API endpoints for the backend services are mentioned along with their details. Explanation of each endpoint and what it does are mentioned:

1. Extract/Fetch all the lists of application servers(this is the first api endpoint it will be there in the dashboard):
   ```bash
   Endpoint: /api/servers/
   Method: GET
   ```
   - This endpoint when requested will return all the lists of servers that are there in the dashboard. The first opening to your dashboard.
   - When requested this endpoint will return the server information like - Server name, server ID, and other server details in JSON format, and this data will be there on the dashboard.

   
2. Now suppose among 10 or 15 servers in the list you click on a particular server to see the list of all services in that particular server. For these, the endpoint will look like:
   ```bash
   Endpoint: /api/servers/<server_id>/services
   Method: GET
   Parameters:
     server_id: ID of the desired server that the user wants to see. ( this is data that needs to be passed as <server_id> is a placeholder.
   ```
   
   - Now suppose, among the list of servers in the dashboard each server will have a unique server ID stored in the database. Suppose the user requests (or clicks on that server) a server with server ID: 7,
   - Now since the user clicked/requested on Server no: 7 then the API from the above endpoint will look like this:
   
   ```bash
   Endpoint: /api/servers/7/services
   Method: GET
   ```
   
   - Note: Here, 7 will be added to the URL when serverID:7 is requested.
   - Now all the list of services from Server no: 7 will be displayed on a webpage.Returns a JSON object containing a list of services running on the specified server, along with other relevant server details.

  
4. This is the endpoint where after the previous request, the list of all services of a server (for example serverID:7) will be displayed along with Start, Stop, and Restart options in the UI.
   ```bash
   Endpoint: /api/servers/<server_id>/services/<service_id>/control
   Method: POST
   Parameters:
      server_id: ID of the server hosting the service.
      service_id: ID of the desired service.
      action: String specifying the desired action ("start", "stop", or "restart").
   ```
  - Here, in this endpoint, we need to pass the server_id, and service_id of a particular service, and the control: i.e, start, stop, or restart will be posted in the payload as a post request.
  - For example, in the serverId:7 there is a service for example named: ABC whose service_id is 5. And you want to 'restart' it by pressing the 'restart' button in the dashboard. The endpoint will look like this:
    ```bash
     Endpoint: /api/servers/7/services/5/control
     Method: POST.
    ```
    Now keep in mind, that we need to post the control/operation whether it's start, stop or restart.
    ```python copy
    Endpoint: /api/servers/7/services/5/control
    Content-Type: application/json
    POST:
    {
      action: "restart"
    }
    ```
    Now each and every endpoint will have a function. For this above mentioned endpoint, the function will look like this:
    ```python copy
    def service_control_view(request, server_id, service_id):
         if request.method == 'POST':
            data = request.data
            data["server_id"] = server_id
            data["service_id"] = service_id

            # Validate the data using the serializer
            serializer = ServiceControlSerializer(data=data)

            if serializer.is_valid():
   
               action = serializer.validated_data["action"]
               if action == 'start':
                  start_process(process_name_or_command)
               elif action == 'stop':
                  stop_process(process_name_or_command)
               elif action == 'restart':
                  restart_process(process_name_or_command)

               return JsonResponse({'status': 'success', 'message': f'Service {action}ed successfully'})
    
           else:
             return Response(serializer.errors, status=status.HTTP_400_BAD_REQUEST)
                 
    ```
   
    






