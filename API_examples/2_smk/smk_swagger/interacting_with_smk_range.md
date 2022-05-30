# Extracting art works from the flourishing period with the SMK API
The period from 1775 to the bombardment of Copenhagen in september 1807 was a time of mercantile flourishing in Denmark. This was due to Denmark's neutrality in the wars between the great powers of the period. Under this neutrality Denmark was able to increase trade making especially Copenhagen traders rich. But let's say that we want to examine this period in art. How do we extract the art works from the this flourishing period that lies within SMK?  
This will be the focus of this lesson, where we will demonstrate how to usi the SMK API swagger interface to construct a call to the API that extracts all the art works in SMK in the period from the first of January 1775 to the 30. september 1807.  
# The SMK swagger interface
When we land on the SMK Swagger interface it looks like this: 
![The landing page of the SMK API swagger site](/instructions/smk_swagger/0_landing.png)

We see that there are several sections (Artworks, Artists IIIF, etc.). These a called endpoints and they are a kind of stands offering different services. At the first one we are served information on Artworks, the next information on Artist and so on. Since we want artworks from the flourishing period we will chose the "Artworks"-stand.  
But there are several options here. In this case we will choose the "/art/search". 
![/art/search landing](/smk_swagger/instruction_pics/1_landing_edited.png)

Folding out an option will give you more detail on what it does:
![What it does](/instructions/smk_swagger/instruction_pics/2_what_it_does.png)
This is exactly what we want in this case: a collection of artworks from the flourishing period with all availble information.  
The next step is to hit the "Try it out"-button: 
![Try it out](/instructions/smk_swagger/instruction_pics/3_try_it_out.png)
This gives you the opportunity to modify all the fields in the formula:
![Trying out the swagger interface](/instructions/smk_swagger/instruction_pics/4_trying_it_out.png)
The different fields can modify your query of the art works. The first field is "keys" which is required. This is the keyword that we will be looking for. Since we are not really looking for anything particular, we want all the works from our period, we will leave the star(it was there by default) in the keys-field. This returns everything. The next field is the output - we will leave that be, but notice how the swagger interface gives information on each field. If nothing is selected we will be served the SMK JSON. We are happy with that for now. There is a lot of fields, which consitutes alot of ways of modifiying the result that the API returns. This is really handy, when you know how the API works and exactly what you need and what you need it for. But for now it is confusing and perhaps even a bit frigthening. But that is okay. Imagine all of thise fields as knobs and switches for you to control the output of the API. Let's keep our objective in mind: extracting all the art works from 1. January 1775 and 30. September 1807. **We need to find the field where you can put in a *range* of dates**.  
The next step is there fore to scroll down the swagger page and find the "range"-field: 
![The Range field](/instructions/smk_swagger/instruction_pics/5_range.png)
There are several things here to note before we interact with the field. The first thing is the formula for defining a range: 
> [field:{start;end}]

This formula will be helpfull when we are going to create our own range in a minute. But before we do that we observe that the info about the range field also lists "Available Ranges" - this is all the things we can put instead of "field" in the formula above. So what ranges do we need in order to find the art works from our period? In this case we will use the range "production_dates_end". This way we are sure that the art works were finished within our period. One could argue that "production_dates_start" would work just as fine, but we wont go into this here.  
The last thing we will note is that we are given an example: 
> [modified:{2019-05-28T09:35:58Z;*}]

In this example the API is finding art works that have been modified in the period from the 28th of May 2019 at 09.35.58 to now. The "to now" is the "*" after the semicolon. It can be substituted by a date. So lets focus on the date format that iniates the range- this gives us the formula for which format the API expects dates to be in: 
>2019-05-28T09:35:58Z
>YYYY-MM-DDTHH:MM:SSZ

This is the ISO 8601 standard of showing dates and time in Universal Coordinated Time. In this standard it is also possible to note time differences, but for now let's keep it in Universal Coordinated Time to keep things simple. 

Let's construct our two times in the correct format:
The beginning of the flourishing period:  

>1775-01-01T00:00:00Z

The end of the flourishing period:
>1807-09-30T00:00:00Z

Let's insert these to time dates into the example before. Observe that we also have changed "modified" to "production_dates_end":
> [production_dates_end:{1775-01-01T00:00:00Z;1807-09-30T00:00:00Z}]

The next step is to feed our new range to the equivalent field in the swagger user interface. This is done by clicking "*Add string item*":
![Add string item](/instructions/smk_swagger/instruction_pics/6_range_add_string.png)

Paste in our range from above in to the field that appears:
![Paste in range](/instructions/smk_swagger/instruction_pics/7_paste_in_range.png)

The next step is to scroll past all the other fields, while not worrying to much about them. Only note that there is alot of ways to query the API and ask for data in all sorts of ways. Scroll all the way down to the "*Execute*"-button and hit it!
![Hit the Execute button](/instructions/smk_swagger/instruction_pics/7_execute.png)

Let the API run for a minute and soon you will see the response below the "*Execute*"-button: 
![The response](/instructions/smk_swagger/instruction_pics/8_response.png)

The first black box is a curl-instruction to be used at a command line. This wont be the focus of this lesson.  
The next black box is the request URL used for accessing the data returned. If you look closely you'll notice our dates of interest in that URL. We will use this URL and discuss it further in the next lesson. For now we move on to the next and biggest black box.  
Under "*Server Response*" we see "*Response body*". This is a neatly formatted preview of data that is found when opening the request URL. By skimming the response, and not worrying to much about what it all means, we see "Den Kongelige Kobberstiksamling". This is a first indication that we are on to something.  
If we study the response closer we see that the first line is "*offset: 0*", which means that that the API starts returning art works from 0. This will make more sense after discussing the next two lines.  
"*rows: 10*" means that we have only ten of the art works from our period in the response. The next line "*found: 5084*" explains that there are 5084 art works within our period.  
So in summary our response has 10 art works out of 5084. But how du we extract all 5084 art work from the flourishing period? This will be the focus of the next lesson. 

In this lesson you have learned how to intereact with a swagger inferface in order to construct call to a API 
