FastAPI  
[https://github.com/zhanymkanov/fastapi-best-practices](https://github.com/zhanymkanov/fastapi-best-practices)  
- routers (use service)  
- service (use db)  
- db  
### general  
```python  
# Makefile  
runserver:  
uvicorn kundi.server.main:app --reload  
testendpoint:  
pytest ./tests/test_endpoints.py  
  
  
app = FastAPI()  
  
origins = [  
"[http://localhost.tiangolo.com](http://localhost.tiangolo.com)",  
"[https://localhost.tiangolo.com](https://localhost.tiangolo.com)",  
"http://localhost",  
"http://localhost:8080",  
]  
  
app.mount("/static", StaticFiles(directory="static"), name="static") #access static files  
  
# dependency  
app.add_middleware(  
CORSMiddleware,  
allow_origins=origins,  
allow_credentials=True,  
allow_methods=["*"],  
allow_headers=["*"],  
)  
  
# @app.middleware("http") sucks because cannot control order  
# async def verify_auth(request: Request, call_next):  
# try:  
# token = oauth2_scheme(request)  
# auth.verify_id_token(token, check_revoked=True)  
# except:  
# HTTPException(status_code=status.HTTP_401_UNAUTHORIZED)  
# response = await call_next(request)  
# return response  
```  
### test endpoints  
```python  
from kundi.server.main import app  
from fastapi.testclient import TestClient  
  
  
client = TestClient(app)  
  
  
def test_read_main():  
response = client.get("/")  
assert response.status_code == 200  
assert response.json() == {"Hello": "World"}  
  
```  
### Auth  
```python  
from [fastapi.security](http://fastapi.security) import OAuth2PasswordBearer  
Token = Annotated[str, oauth2_scheme]  
  
// ==================== 1.USE DEFAULT TOKEN (doc has this)  
@app.post("/v1/signin")  
def signin(token: Annotated[str, Depends(oauth2_scheme)]):  
try:  
decoded_jwt = auth.verify_id_token(token, check_revoked=True)  
return decoded_jwt  
except:  
raise HTTPException(status_code=status.HTTP_401_UNAUTHORIZED)  
  
  
// =================== 2.USE DEPENDENCY (docs dont have but more reusable)  
async def parse_token(token: Annotated[str, oauth2_scheme]):  
try:  
decoded_jwt: dict = auth.verify_id_token(  
id_token=token, check_revoked=True)  
  
return decoded_jwt  
except:  
return None  
@ app.get("/v1/sets/{set_id}/cards")  
def get_cards(user: Annotated[dict, Depends(parse_token)], set_id: str, review: bool = False):  
if user == None:  
raise HTTPException(status_code=401)  
return db_get_cards(user["email"], set_id, review)  
  
```  
### typing  
```python  
Card(**dict)  
# must have, different typing for different purpose: CreateCard and Card different  
class CreateCardPayload(BaseModel):  
word: str  
definition: str  
  
class Card(BaseModel):  
card_id: str  
word: str  
definition: str  
review_counts: int = 0  
review_due: str  
box_id: int = 0  
```  
### structuring  
```python  
#routers/user  
from fastapi import APIRouter  
  
router = APIRouter(  
prefix="/items",  
tags=["items"],  
dependencies=[Depends(get_token_header)], #  
responses={404: {"description": "Not found"}}, # define response for status code  
)  
@router.get("/users/", tags=["users"])  
async def read_users():  
return [{"username": "Rick"}, {"username": "Morty"}]  
  
#main.py  
app.include_router(users.router)  
app.include_router(items.router)  
```