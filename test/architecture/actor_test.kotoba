(ns architecture.actor-test
  (:require [clojure.test :refer [deftest is testing]]
            [architecture.store :as store]
            [architecture.advisor :as advisor]
            [architecture.actor :as actor]))

(defn- fresh-graph []
  (let [st (store/mem-store)]
    (store/register-project! st {:project-id "proj-1" :client-id "client-1" :name "Community Garden"})
    (actor/build-graph
     {:store st
      :advisor (advisor/mock-advisor)})))

(deftest end-to-end-draft-design-concept
  (let [g (fresh-graph)
        result (actor/run-request!
                g
                {:project-id "proj-1" :op :draft-design-concept :stake :low}
                {}
                "thread-1")]
    (is (= :done (:status result)))
    (is (empty? (:frontier result)))))

(deftest end-to-end-escalation-on-environmental-concern
  (let [g (fresh-graph)
        result (actor/run-request!
                g
                {:project-id "proj-1" :op :flag-environmental-compliance-issue :stake :high}
                {}
                "thread-2")]
    (is (= :interrupted (:status result)))
    (is (seq (:frontier result)))))

(deftest end-to-end-hard-reject-on-unregistered-project
  (let [g (fresh-graph)
        result (actor/run-request!
                g
                {:project-id "no-such-project" :op :draft-design-concept :stake :low}
                {}
                "thread-3")]
    (is (= :done (:status result)))
    (is (empty? (:frontier result)))))
