# Network Specification  #

The following document covers how 1.7.5, 1.8 an 2.3.0+ versions of AO connect and communicate with servers.

## Legend ##

AO = Attorney Online  
char = character, as in the game character, not the char datatype  
type(x) = an array of type separated by x: typextypextype
C = client  
S = server  
[x] = x is an optional argument(usually used by version 2.3.0+)  
{x} = x is encrypted using the "fantacrypt" algorithm  

# Handshake #

The handshake is largely the same regardless of server and client version.  
  
C: (ordinary TCP handshake)  
S: **decryptor#{<decryptor: hex>}#%** (the argument is decrypted using **unsigned integer** 322 as decryption key)  
(all headers the from the client are encrypted from here on)*  
C: **HI#<unique_hardware_id: string>#%**  
S: **ID#<player_id: int>#<software: string>[#<version: int.int.int>]#%** (player id is largely unused)  
[C: **ID#<software: string>#<version: int.int.int>#%**]  
S: **PN#<player_count :int>#<max_players: int>#%**  
[S: **FL#<feature1: string>#...#<featureN: string>#%**]  
  
*: not always true, if the server sends noencryption as an argument in the FL packet, headers are no longer encrypted(2.3.1+ only)  

# Loading #

Loading is the process where the server sends lists of its characters, music and sometimes evidence to the client.

## Loading 1.0 ##

Used by versions 1.8 and 1.7.5, this loading is very inefficient and suffers heavily from latency. Can also be used by 2.3.0+ in cases where the server does not support fast loading.  
  
C: **askchaa#%**  
S: **SI#<char_list_length: int>#<music_list_length: int>#<evidence_list_length: int>#%**  
C: **askchar2#%**  
S: **CI#<char_id: int>#<char_name: string>&<description: string>&<?: int>&<evidence: int(,)>&&<?: int>&#...#%**  (1-10 in every packet, continued in the ...)    
C: **AN#1#%**  
S: **CI#...#%**  
C: **AN#2#%**  
...  
S: **EI#<evidence_id: int>#<evidence_name: string>&<evidence_description: string>&1&<evidence_image_name: string>&##%**  (evidence loads one at a time)  
C: **AE#1#%**  
S: **EI#...#%**  
C: **AE#2#%**  
...  
S: **EM#<music_id: int>#<music_name: string>#...#% (music is also loaded in packs of 10)**  
C: **AM#1#%**  
S: **EM#...#%**  
C: **AM#2#%**  
...  
S: **CharsCheck#<char_id: int>#...#%** (same size as character list, 0 = free, -1 = taken)  
S: **OPPASS#{<mod_pass: hex>}#%** (an obsolete artifact of the disastrous security of old AO servers, still used on AO clients 1.8 and lower to display a guard checkbox in the client)  
S: **DONE#%**  

A sample can be found here(todo: add link)
  
## Loading 2.0 ##

Designed to be *much* faster than Loading 1.0, most noticably on slow connections.  
  


  
