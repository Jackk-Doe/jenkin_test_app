# jenkin_test_app

Build :
docker build -t jackkdoe/jenkin_test_app .

Push :
docker push jackkdoe/jenkin_test_app

Test run :
docker run -p 3000:3000 jackkdoe/jenkin_test_app