# 레시피 데이터 예시################################
recipe = {"감자 짜글이":
              {"재료": ["감자", "양파", "대파", "청양고추", "고춧가루", "고추장", "간장", "스팸"],
               "레시피": ["1. 채소들을 썰어준다.", "2. 스팸을 으깨준다.", "3. 준비된 재료들을 냄비에 넣고 간장과 물을 넣는다.", "4. 골고루 섞어서 끓여준다."]},
          "대패삼겹 두부조림":
              {"재료": ["대패 삼겹살", "두부", "대파", "청양고추", "간장", "설탕", "고춧가루"],
               "레시피": ["1. 준비된 재료를 썬다.", "2. 냄비에 재료들을 담아준다", "3. 양념과 물을 넣고 조려준다."]},
          "오므라이스":
              {"재료": ["계란", "당근", "양파", "스팸", "대파", "밥"],
               "레시피": ["1. 준비된 재료들을 잘게 썰어준다.", "2. 대파를 볶아준다", "3. 나머지 채소도 넣어 볶아준다.", "4. 소금간을 한 뒤 밥을 넣어 볶아준다.",
                       "5. 계란을 부치고 볶아둔 밥을 넣고 말아준다."]},
          "볶음밥":
              {"재료": ["당근", "양파", "대파", "밥"],
               "레시피": ["1. 준비된 재료들을 잘게 썰어준다.", "2. 대파를 볶아준다", "3. 나머지 채소도 넣어 볶아준다.", "4. 소금간을 한 뒤 밥을 넣어 볶아준다."]}
          }

##############################################

while True:
    print("1. 레피시 검색")
    print("2. 종료")
    choice = input(" 선택 >>  ")  # 선택한 메뉴 실행
    print()

    if choice == "1":  # '1. 레시피 검색'의 경우
        while True:
            recipe_igd = input("사용할 재료를 입력해주세요(쉼표로 구분) : ")  # 당근, 양파, 대파, 밥 처럼 쉼표로 구분하여 입력
            recipe_igd = recipe_igd.split(",")  # 문자열을 쉼표 단위로 나눠 리스트로 저장
            for idx, r in enumerate(recipe_igd):
                recipe_igd[idx] = r.strip()  # 각 단어의 양쪽 공백 제거 >> " 당근 "이면 "당근"으로

            if len(recipe_igd) < 3 or len(recipe_igd) > 10:  # 입력된 재료가 3개미만이거나 10개 초과면 오류후 재입력
                print("재료가 너무 적거나 많습니다.")
            else:
                break
        print()

        while True:
            print("    1. 등록한 재료만 이용")
            print("    2. 추가 재료까지 이용")

            search_type = input("     선택 >>  ")  # 원하는 메뉴 선택
            print()

            if search_type == "1" or search_type == "2":  # 1번이나 2번이면 선택완료
                break
            else:
                print("잘못된 선택입니다.")  # 다른번호면 재선택

        if search_type == "1":  # 등록한 재료만 이용
            recipe_title = []  # 레시피 제목들 저장 리스트
            recipe_rate = []  # 재료 일치율 저장 리스트

            for k in recipe:  # 저장된 모든 레시피 탐색
                count = 0
                for i in recipe[k]["재료"]:
                    if i in recipe_igd:  # 저장된 재료가 입력한 재료에 있으면 count 1 증가
                        count = count + 1

                cmp_rate = count / len(recipe[k]["재료"])  # count를 현 레시피의 재료 개수로 나누어서 비율 계산

                if cmp_rate > 0.85:  # 0.85 이상의 일치율을 가진 레시피만 저장
                    recipe_rate.append(cmp_rate)
                    recipe_title.append(k)

                for i in range(len(recipe_rate) - 1):  # 정렬알고리즘(버블 소트)으로 정렬  (정확도순)
                    for j in range(len(recipe_rate) - i):
                        if j == 0:
                            continue
                        if recipe_rate[j - 1] < recipe_rate[j]:
                            recipe_rate[j - 1], recipe_rate[j] = recipe_rate[j], recipe_rate[j - 1]
                            recipe_title[j - 1], recipe_title[j] = recipe_title[j], recipe_title[j - 1]

            for r in recipe_title: # 저장된 내용 출력
                print(r)

        elif search_type == "2":  # 추가 재료까지 이용
            recipe_title = []  # 레시피 제목들 저장 리스트
            recipe_rate = []  # 재료 일치율 저장 리스트

            for k in recipe:  # 저장된 모든 레시피 탐색
                count = 0
                for i in recipe_igd:
                    if i in recipe[k]["재료"]:  # 입력한 재료가 저장된 재료에 있으면 count 1 증가
                        count = count + 1

                cmp_rate = count / len(recipe[k]["재료"])  # count를 현 레시피의 재료 개수로 나누어서 비율 계산

                if cmp_rate > 0.15:  # 0.15 이상의 일치율을 가진 레시피만 저장
                    recipe_rate.append(cmp_rate)
                    recipe_title.append(k)

                for i in range(len(recipe_rate) - 1):  # 정렬알고리즘(버블 소트)으로 정렬  (정확도순)
                    for j in range(len(recipe_rate) - i):
                        if j == 0:
                            continue
                        if recipe_rate[j - 1] < recipe_rate[j]:
                            recipe_rate[j - 1], recipe_rate[j] = recipe_rate[j], recipe_rate[j - 1]
                            recipe_title[j - 1], recipe_title[j] = recipe_title[j], recipe_title[j - 1]

            for r in recipe_title:
                print(r, "  ", "필요한 추가 재료 : ", end='')
                for m in recipe[r]["재료"]:
                    if m not in recipe_igd:
                        print(m, " ", end='')
                print()

        print()

        recipe_k = input("레시피를 확인하실 메뉴를 입력하세요 : ")  # 레시피 제목 입력해서 레시피 출력
        for r in recipe[recipe_k]["레시피"]:
            print(r)

        print()
    elif choice == "2":  # '2. 종료'의 경우
        break
    else:
        print("잘못된 선택입니다.")
        print()
