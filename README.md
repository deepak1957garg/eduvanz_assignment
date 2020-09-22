An assignment by Eduvanz

Problem : Design solution for a given problem
		 
		  A website for event registration, with admin view who can view all registered users with their details and get stats as per data filled in form.

		  
Solution :

Actors : Users (who registered), Admins (Organisers of events)

		User : users comes on site -> view event details -> fill the form
		Admin : see list of users, search users, view user details, see stats (analytics)


		Keeping in minds future Considerations:

		Constraints (assumed) : 1) There can be more events in future so design took care without changing much
		  						  2) Numbers of users in any events can be in hundreds or may be 100 thousands people event running for multiple days 			

		  
		Data Models :   1) Events (event_id,event_name,event_location,event_date,event_description,registration_criteria)
		                2) Sub Events (sub_event_id,event_id,sub_event_name,schedule)
		                3) Users (user_id,name,age,dob,profession,locality,address,joined_on,email,mobile_no,social_id,updated_on)
		                4) Events_registration (event_id,user_id,role,num_guests)

		                5) Stats (id,type{age,locality,num_guests,profession},sub_type{0-13,18-25,student,professional},count,last_updated) 
		                6) Search_keyword_master (keyword_id,keyword(can be one word or combination of multiple words),loc)
		                7) Search_keyword_index (keyword_id,user_id)

		                8) Search_index_autocomplete (id,sub_keyword(e.g. aa,ami),keywords(json of keywords)) 


		Rest Apis - Post (www.xyz.com/user) (for adding a user)
					Get  (www.xyz.com/search) (for searching a user location and user name)
					Get  (www.xyz.com/stats) (for getting stats)


		Search Autocomplete should be stored in local storage of browser after the request completed with an expiry, so next time if request comes it serves from local storage if not found then only go to server.

		If traffic is huge, we should consider use process workers for updating stats and building search index. We should keep requests in queues like {user_add,{user_id}} so that response will be fast. And process worker keep checking queue after an interval of time like 30 sec. And found something in queue process it. (if huge traffic we can use any messaging server like kafka)


		Classes - 1) DataLayer which interact with databases or caching servers or in future required with third party apis.
				  2) UserRegistration (handles login,signup of users and store data through Data layer)
				  3) StatsHandler (will get all stats from stats table and send to view(html))
				  4) SearchResults (will get serach results)
				  5) View(pages) can be graphical using any data tools like D4.js etc 


		On web pages : 1) validation should be done
		               2) shows content as per user roles
		               		  