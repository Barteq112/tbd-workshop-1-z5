IMPORTANT ❗ ❗ ❗ Please remember to destroy all the resources after each work session. You can recreate infrastructure by creating new PR and merging it to master.
  
![img.png](doc/figures/destroy.png)

1. Authors:
Group: ***5***
   Bartosz Sędzikowski
   Julian Żebrecki
   Oskar Gorgis
   ***https://github.com/Barteq112/tbd-workshop-1-z5/blob/master/tasks-phase1.md***
   ***https://github.com/Barteq112/tbd-workshop-1-z5/***   
3. Follow all steps in README.md.

4. From avaialble Github Actions select and run destroy on main branch.
   
5. Create new git branch and:
    1. Modify tasks-phase1.md file.
    
    2. Create PR from this branch to **YOUR** master and merge it to make new release. 

  
    ![img.png](images/Screenshot1.png)
    ![img.png](images/result_1.png)

6. Analyze terraform code. Play with terraform plan, terraform graph to investigate different modules.

   Moduł gcr
   
   Cel:
   Udostępnienie rejestrów obrazów Dockera w GCP wraz z włączeniem wymaganych API.

   Tworzone zasoby:
   - google_project_service.api - Ten zasób włącza Google Artifact Registry API w danym projekcie GCP i nadaje lokalną nazwę "api",
tak aby można było tworzyć repozytoria Dockera i przechowywać obrazy kontenerów.
API pozostanie aktywne nawet po usunięciu infrastruktury Terraformem.
    - google_artifact_registry_repository.registry - Tworzy repozytorium Artifact Registry w regionie Europe (pod warunkiem że zostało utworzone api), z włączonym formatem Docker i pozwala nadpisywać tagi obrazów. Dodaje mu opis "TBD Docker repository" i ustawia typ na Dockera, któremu pozwala na nadpisywanie tagów.

   Zmienne na wejściu:
   - project_name (nazwa projektu)
   - location (lokacja, domyślnie ustawiana na EU)
  
   Wymagania:
     - Terraform - 1.11.0
     - google - 5.44.0

   Wyjścia:
     - registry_hostname - zwraca nazwę rejestru kontenerów GCP
     
   ![img.png](images/Screenshot4.png)
   
7. Reach YARN UI
   (wykonujemy wszystko w terminalu linux)
   Po poprawnym zalgowoaniu używając komendy: gcloud auth login
   Oraz po zakończeniu wykonania Terraform, należy wpisać poniższą komendę.
   Komenda:
   gcloud compute ssh tbd-cluster-m --project tbd-2025z-5 --zone europe-west1-b --tunnel-through-iap -- -N -L 8088:localhost:8088

   ![img.png](images/Screenshot2.png)
   ![img.png](images/Screenshot3.png)
   
9. Draw an architecture diagram (e.g. in draw.io) that includes:
    1. Description of the components of service accounts
    2. List of buckets for disposal
    
    ***place your diagram here***

10. Create a new PR and add costs by entering the expected consumption into Infracost
For all the resources of type: `google_artifact_registry`, `google_storage_bucket`, `google_service_networking_connection`
create a sample usage profiles and add it to the Infracost task in CI/CD pipeline. Usage file [example](https://github.com/infracost/infracost/blob/master/infracost-usage-example.yml) 

   ***place the expected consumption you entered here***

   ***place the screenshot from infracost output here***

11. Create a BigQuery dataset and an external table using SQL
    
    ***place the code and output here***
   
    ***why does ORC not require a table schema?***

12. Find and correct the error in spark-job.py

    ***describe the cause and how to find the error***

13. Add support for preemptible/spot instances in a Dataproc cluster

    ***place the link to the modified file and inserted terraform code***
    
14. Triggered Terraform Destroy on Schedule or After PR Merge. Goal: make sure we never forget to clean up resources and burn money.

Add a new GitHub Actions workflow that:
  1. runs terraform destroy -auto-approve
  2. triggers automatically:
   
   a) on a fixed schedule (e.g. every day at 20:00 UTC)
   
   b) when a PR is merged to main containing [CLEANUP] tag in title

Steps:
  1. Create file .github/workflows/auto-destroy.yml
  2. Configure it to authenticate and destroy Terraform resources
  3. Test the trigger (schedule or cleanup-tagged PR)
     
***paste workflow YAML here***

***paste screenshot/log snippet confirming the auto-destroy ran***

***write one sentence why scheduling cleanup helps in this workshop***
